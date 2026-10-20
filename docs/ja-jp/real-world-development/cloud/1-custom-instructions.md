---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot にタスクの内容だけでなく、実行方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて比較しにくいためです。この演習の後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認できます。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には常にユニットテストが必要です。
- UI はダークモードにし、モダンな見た目にします。
- コードには TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、これらの実践に沿ったタスクを Copilot が実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot が理解できるように、コンテキストと好みを伝えられます。優先するコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体または特定の種類のファイルに指示を適用できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送られる単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどのチャットや CLI 要求に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指針を示すためのファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成タスクなどを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成についてすべて説明するのはこの演習の範囲外ですが、サンプルプロジェクトの例は代表的な方法を示しています。

- `copilot-instructions.md` には、開発内容、プロジェクト構造、全体的なコーディング標準など、プロジェクト全体の指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別やタスク向けの指針を記載します。
- 自然な言葉で明確に書き、コードの良い例と悪い例を示します。

指示ファイルの作成方法は1つではありません。AI の使い方と同様、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。操作方法は環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。この演習では、該当する操作を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。中核となる `copilot-instructions.md` が1つあり、さまざまなタスク向けの `*.instructions.md` ファイルも用意されています。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示の適用対象をリポジトリルートからの glob として指定しています。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾にある、`unit-tests.instructions.md` などのほかの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考にする例を Copilot に示せます (記載されているパスはリポジトリルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ求められていません。次の手順で TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、コードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーを追加するという、プロジェクト全体のルールを追加することです。この演習の後半で Issue を割り当てると、cloud agent がこのルールを参照します。

編集の前にブランチを作成します (cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは変更を整理してコミットします)。

1. Codespace のターミナルで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイルコメントヘッダーと TSDoc ドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` としても、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` としても作成できます。今回追加したドキュメントコメントのルールなど、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作られます。cloud agent が指示を利用できるように、変更を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` にプルリクエストを作成してマージします。次の演習で Issue を割り当てたときに、cloud agent がこの指示を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` にコミットしても構いません。ここでブランチを作るのは、実際のプロジェクトに近い方法で作業するためです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストにその標準が反映されているか確認します。

次は [最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
