---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに確認したように、コードを書く以外にも、データや外部サービスを利用し、Copilot に追加の自動化機能を提供する必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能以外のツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには実際に UI を操作する必要があります。Copilot がユーザーのように Web サイトを使えるようにし、変更をより確実に作成できるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信する方法を提供します。MCP を使うと、エージェントは最新情報にアクセスし、ユーザーに代わって操作を実行できます。

ツールやリソースには、AI エージェントと外部ツールやサービスの橋渡しをする MCP サーバーを介してアクセスします。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

既存の MCP サーバーの例を示します。

- **[GitHub MCP Server][github-mcp]:** GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]:** Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP registry][mcp-registry] で、エコシステム内のサーバーを見つけたり、コントリビュートしたりできます。

> [!CAUTION]
> MCP サーバーも、プロジェクトのほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を考慮します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるように追加します。

> [!NOTE]
> Playwright MCP が Copilot CLI ですでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を調べるよう Copilot に依頼する

前の手順では、機能が期待どおりに動作することを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自分で判断して使うため、明示的に指定する必要はありません。使うべきサーバーが分かっている場合は、指定すると結果がより安定し、トークンの節約にもなります。

2. Copilot がブラウザーを操作して機能を確認する様子を見ます。
3. 報告内容を読み、動作に問題がないことを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止した後にレポートを作成します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- まだ利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールをまとめて使う [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
