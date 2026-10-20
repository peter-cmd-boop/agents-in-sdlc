---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがその例です。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に行います。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。こうした作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- Issue の範囲を明確にする重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの初期版を作成した委託先は、ドキュメントを十分に整備していませんでした。まず、アプリケーションのエクスポートされた関数すべてに TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリーの別のゲームを表示すれば、閲覧を続けてもらえます。洗練された実装はまだ必要ありませんが、UX の受け入れテストに使えるものが必要です。この課題は現在ブロッカーですが、優先度の高い Issue がほかにもあります。

どちらも優先度が下がりやすいタスクの例であり、Copilot cloud agent に任せるのに適しています。cloud agent が非同期で作業している間、開発者はほかの作業に集中し、後で完了した内容をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。実行中または完了後にセッションを確認できます。解決案をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える必要がなくなるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot をガイドする方法を確認しました。すでに `.github/copilot-instructions.md` を使ったことがある場合は、その効果を見たことがあるでしょう。これから cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で効果を確かめられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるときは、生成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は適切な状態を保つための環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェアエンジニアと同じように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を用意すると、作業開始前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。ラボの流れと学習を支援するため、このファイルは事前に構成されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各ステップ*で必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体が Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションし、ビルドやテスト用のデータをシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でワークフローを実行し、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古い、または不足しているプロジェクトは少なくありません。放置されがちなこの種の技術的負債は、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に適しているため、このタスクを Copilot cloud agent に割り当てましょう。バックグラウンドで必要なドキュメントを作成し、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧にない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成します。

> [!NOTE]
> Copilot が PR に To Do リストを表示するまで数分かかる場合があります。Copilot は環境を作成して (前述のワークフローを実行して)、プロジェクトを分析し、タスクの進め方を決めています。

13. リストと実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されるまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決していく様子を確認します。コードを調査して状態を理解し、考えながら適切な計画を決め、コードを作成する過程を見られます。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は非同期でタスクを実行できることです。この利点を活用して、別のタスクも cloud agent に割り当てた後、アプリケーションの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けの関連ゲーム表示に多くの時間がかからない場合でも、その時間を別のタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Issue に取り組んでいることを目の絵文字で示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューしましょう。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。Issue を割り当てると、Copilot が非同期で作業できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
