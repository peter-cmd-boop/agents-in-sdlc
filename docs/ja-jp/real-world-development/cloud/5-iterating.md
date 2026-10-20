---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験を改善し機能を追加する複数のタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザインチームが改善を続けられる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスを実行します。チーム全体でレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全性を確保するための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードの作業に使うブランチに限って書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも含まれます。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面でレビューします。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は Copilot の完了を待ちます。数分かかることがあるので、休憩したり学んだ内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメントコメントなど、新たに更新されたコードとドキュメントを確認します。変更内容は状況によって異なります。

   変更を確認するときは、TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーに注目します。これらは演習 2 の開始時に確認したカスタム指示ファイルから反映されています。

6. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と協力する方法は、作業を依頼するだけではありません。チームのメンバーと同様、PR のコメントやコードへのインラインコメントで Copilot をメンションできます。Copilot はコメントを読み、新しいセッションを開始して対応します。結果は決定論的でないため、依頼文を一律には指定できません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、そのファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の案などを使い、生成されたドキュメントを変更するようコメントを追加します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ります。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで改善を続けてください。完了したら、ドラフト PR をレビュー準備完了に変更し、`main` ブランチにマージできます。

![PR をレビュー準備完了に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチをチェックアウトし、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent にコメントを追加して反復的に更新を依頼できます。たとえば、関連ゲームの表示数を調整したり (この Issue はカスタム指示を更新する**前**に割り当てたため)、コメントヘッダーと TSDoc ドキュメントコメントを追加したりできます。**Conversation** タブに新しいコメントを追加すると、Copilot がその内容を確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。数分かかることがあるため、完了を待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するためのローカルストレージの使用。
   - ハイコントラストとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容に問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチをチェックアウトし、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動してブラウザーでハイコントラストとライトモードの切り替えをテストし、期待どおりに動作することと、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装をガイドしたことを確認できます。アクセシビリティ上の懸念や改善案があれば、前の PR と同じようにコメントで **@copilot** をメンションして変更を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せ、自分は別の作業に取り組むのが効果的です。この習慣を身に付けるため、リポジトリにいくつか Issue を作成し、Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。次の作業を行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認**し、cloud agent の出力に反映されることを確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使用**し、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使用**し、実行中のアクセシビリティセッションを監視して指示しました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` をメンションして変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスはこれで完了です。ほかのハーネスでは、別の環境から関連するシナリオに取り組めます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` ブリッジを使って、レビュー済みの変更を2つターミナルからリリースします。

自分のリポジトリで、次の案も試してください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てます。
- 別の領域向けにカスタムエージェントを作成します。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
