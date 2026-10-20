---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを自分で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーによって Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動テストするよう依頼します。
- ブランチを公開し、フィルター機能の PR を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にある機能にアクセスできると、さらに強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準です。

![エージェントモードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含むコンテキスト、LLM、ツール間の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。リソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わる操作を実行できます。

ツールやリソースには、AI エージェントと外部ツールやサービスの橋渡しをする MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールとの通信を管理し、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。リポジトリの作成や更新、Issue と PR の管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** Playwright を使ったブラウザー自動化機能を提供します。Web ページの移動、フォームの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、コントリビューションを促進するために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP サーバーも、プロジェクトのほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証し、セキュリティへの影響を考慮します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読めました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルでのインストールは不要です。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能をテストするのに利用できます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、画面を表示せずにブラウザーを実行します。Codespace には表示するデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストにすでに Playwright を使っているため、必要なブラウザーもインストール済みの場合がほとんどです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認した場合は、設定を確認して信頼を選択し、起動を許可します。

手動での起動は省略できます。その場合は、次の手順で Copilot がツールを初めて必要としたときにサーバーが起動し、信頼の確認が表示されます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。エージェントはページを開き、移動し、フィルターを適用して、結果を読み取り、確認した内容をまとめられます。会話を離れずに機能が期待どおり動作するかを確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベルの付いた構造化要素をエージェントが基にして推論します。これは支援技術の動作と同じで、簡単な機能テストをアクセシビリティの確認にも役立てられます。

サーバーを接続してアプリを起動した状態で、次のプロンプトを使い、作成したフィルター機能を Copilot にテストしてもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ条件と照らし合わせます。問題があれば、追加で質問するか、PR を作成する前にコードを修正するよう依頼します。

> [!NOTE]
> テストにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送信する前にもう一度起動します。Playwright MCP サーバーを初めて使うときは、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したら、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Source Control パネルに Copilot が行った変更が表示されている画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に Issue を探してもらい、関連付けた PR を作成してもらいます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. エージェント選択メニューで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使えるようになります。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub ツールを初めて使うときに認証を求められたら、画面の指示に従います。
5. Copilot が Issue を検索して、正しいものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- PR を作成する前にブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェント][next-lesson]を作成して、アクセシビリティレビューなどに特化した作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
