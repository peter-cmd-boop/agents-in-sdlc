---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業をより効果的に行うための指示、ベストプラクティス、コーディングパターン、ドメイン知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを保つテストエージェントなどです。

カスタムエージェントはプロジェクト内の `.github/agents`、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの振る舞い、専門知識、指示を定義する Markdown プロンプトがあります。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一環としてスキルや別のエージェントを調整できます。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティのガイドラインに沿ってサイトを確認、更新します。その作業で、プルリクエストのワークフローやテストの実行と管理などのスキルを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが十分でなく、読みにくいという意見が寄せられました。デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を求めています。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Codespace に戻り、ターミナルで `main` からアクセシビリティ作業用の新しいブランチを作成します (演習 3 のフィルター機能の PR とは分けておきます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成における中核的な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` にある各カスタムエージェントが Copilot Chat ビュー下部のエージェント選択メニューに表示されます。カスタムエージェントを選ぶと、その指示とツールを使うチャットセッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します (すでに選択されている場合はそのまま進みます)。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを開きます。既定では、現在のエージェントとして **default** が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトファイルに適用します。各編集はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが変更したファイル数が **Files changed** に反映されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを実際に変更します。

次の演習では、この作業の進行中にレビューして指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域向けに調整された AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をガイドできます。

次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視し、方向付けます][next-lesson]。変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
