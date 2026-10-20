---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報にアクセスできるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成でどのような指針に従うかを伝えます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルター機能の最初のデータ処理部分 (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されるコードが新しい標準を採用することを確認します。
- 指示の変更とフィルター機能の基礎部分を `main` にコミットし、次の演習で Copilot が使えるようにします。

> [!CAUTION]
> 生成コードが指定した標準から外れることもあります。Copilot は非決定論的です。この演習では、指示を更新した後の動作の**傾向**を確認します。出力が一字一句同じになることを期待しないでください。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には常にユニットテストが必要です。
- UI はダークモードにし、モダンな見た目にします。
- コードには TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、これらの実践に沿ったタスクを Copilot が実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot が理解できるように、コンテキストと好みを伝えられます。優先するコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体または特定のファイルに指示を適用できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送られる単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどの要求に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けの指針を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

この演習で指示ファイル作成の詳細をすべて扱うことはできませんが、サンプルプロジェクトの例は代表的な方法を示しています。

- `copilot-instructions.md` には、開発内容、プロジェクト構造、全体的なコーディング標準など、プロジェクト全体の指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別やタスク向けの指針を記載します。
- 自然な言葉で明確に書き、コードの良い例と悪い例を示します。

指示ファイルの作成方法は1つではありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。操作方法は環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当する演習で手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、指示ファイルがすでに含まれています。変更による効果を確認する前に、既存のファイルを見ておきましょう。

1. Codespace に戻ります。
2. Codespaces エディター (ターミナルではありません) で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。これらの指示は、このリポジトリで Copilot とやり取りするすべての場面に適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を glob で指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、コード生成時に参照する例を Copilot に示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、改善できる箇所があります。中核となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の項目として次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を更新した後、フィルター機能のコンテキストを失わずに新しい指示を読み込めるよう、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   求められたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明しているか確認します。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同様にソース管理で管理します。リポジトリに変更をコミットしてプッシュしましょう。

1. 新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
2. ターミナルで、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎部分で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成してバックログのドキュメント標準に関する Issue と関連付けます。GitHub ツールはプロジェクトですでに接続されているので、Copilot に Issue を探してもらい、PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot に GitHub で操作を実行させます。プロジェクトテンプレートでは、GitHub ツールが Codespace にあらかじめ接続されています。初めてツールを使うときは、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続に使われる Model Context Protocol を確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。説明には指示の更新と publisher ヘルパーの基礎部分の両方を含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成します。
5. Copilot に代わって GitHub 操作を実行するよう **Allow** を求められた場合は、操作内容を確認してから適切に許可します。
6. PR が作成されたら、Copilot に PR をマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェルコマンドの実行を許可するよう求められた場合は、操作内容を確認してから適切に許可します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の変更とヘルパーの基礎部分をコミットし、次の演習で利用できるよう `main` にマージしました。

次は[エージェントモード][next-lesson]で指示を活用し、コードベース全体に新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
