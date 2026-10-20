---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行する必要がある場合は、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]は、どのコードを作成するかだけでなく、どのように構成するかを伝えます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに及ぼす効果を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- コメントには、コードの内容を言い換えるのではなく、目的や分かりにくい判断の理由を記載します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。`db` 引数を注入する場合は、その引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載します。関連コードを変更した場合は、コメントも最新の状態に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って、こうした実践に沿ったタスクを Copilot が実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot が理解できるように、コンテキストと好みを伝えられます。優先するコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体または特定のファイルに指示を適用できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**の要求で Copilot に送られます。ほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けの指針を記載します。

> [!NOTE]
> ほかの指示形式への対応は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、指示ファイルがすでに含まれています。変更の効果を見るために、まず既存の指示を確認します。

1. Codespace に戻ります。
2. Codespaces エディター (ターミナルではありません) で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、このリポジトリで Copilot とやり取りするすべての場面に適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、適用対象のファイルを決める `applyTo` フロントマターを確認します。
6. `.github/instructions/drizzle.instructions.md` にあるほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分割し、Copilot に参考例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、生成する TypeScript に TSDoc コメントを追加するため、中核となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します (35行目付近)。
2. 最後の箇条書きとして次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

## 更新した指示を適用する

Copilot CLI は会話を開始するときにリポジトリの指示を読み込みます。フィルター機能のコンテキストを維持したまま新しい指示を読み込めるよう、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指示をコードに反映するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正しく説明しているか確認します。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに伝わる仕組みを確認しました。
- 指示ファイルを更新してコーディング標準を設定しました。
- 指示ファイルがコードに及ぼす効果を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
