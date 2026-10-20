---
title: "演習 2 - Copilot エージェントモードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションを少し変更するだけでも、複数のファイルの更新やテストの実行が必要になることがあります。開発者は必要なファイルを探し、変更を加え、テストを実行し、デバッグし、更新漏れのファイルを見つけて修正する、といった作業を繰り返します。

このような作業に役立つのが Copilot エージェントモードです。

エージェントモードは IDE 内でより自律的に動作します。開発者と同じようにプロジェクト構造を調査して必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は用途が異なります。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話が可能です。Cloud agent はチームの一員のように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して連携した変更を行う方法。
- UI とデータ層にまたがる新機能を実装する方法。
- Copilot が生成した変更とテストをレビューし、コードベースにマージする前に確認する方法。

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリーとパブリッシャーで絞り込めるようにします。前の演習で publisher ヘルパーを追加しました。ここでは、Copilot エージェントモードを使ってデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。各ページは Astro のフロントマターで Drizzle ORM を使ってローカルの SQLite データベースに直接問い合わせます。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能には標準の DOM API を使う小規模でスコープを限定した Astro `<script>` を使います。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後、`main` に戻っていることを確認して Codespace を開きます。
2. Codespace 内の新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. Web サイトを開くには、ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択します。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。Codespace 内で Web サーバーを実行するためのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、その機能を確認します。主な機能は次のとおりです。

- **ホームページ:** ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。必要な場合は各手順で指定します。

Web サイトの初期実装は動作しますが、機能を追加して改善しましょう。プロジェクトをテンプレートから作成したときに、Issue のバックログも自動的に作られています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します。**Agent** は自律的に計画を立て、複数のファイルを更新し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、体験をできるだけ一定にする目的で手順に記載しています。LLM は確率的に動作するため、提案が記載内容と異なる場合があります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使うことがありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーにとって有用なものを基準に Issue に優先順位を付けます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業にコンテキストを提供する指示ファイルを確認しましょう。この演習では、Web サイトへの機能追加の進め方が記載された [ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、さまざまなファイル種別向けの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用可能な小さなまとまりに分割し、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3つを更新する必要があります。

- データ層 (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功を確認する必要があります。Copilot エージェントモードにこれらの作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、UI の指示ファイルを追加します。

   ![UI の指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを特定する様子を確認します。データ層のヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査してファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては数分かかります。その間、コードエディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、変更を受け入れるか元に戻すか選べます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、一部のテストが失敗したりする場合があります。Copilot はエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードも、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに新機能を追加しました。次のことを確認しました。

- GitHub Copilot エージェントモードで UI とデータ層をまたぐ機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して連携した変更を行う方法。
- コードベースにマージする前に、生成された変更とテストをレビューする方法。

次に、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加演習 — ページ分割を実装する

ゲーム一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだスキルを使い、Copilot にページ分割を実装するよう依頼してみましょう。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望どおりの実装になるよう、必要な指針を含めたプロンプトを作成します。
- 変更を依頼したりコンテキストを追加したりして、Copilot と反復する必要がある場合があります。Copilot を使うときの通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
