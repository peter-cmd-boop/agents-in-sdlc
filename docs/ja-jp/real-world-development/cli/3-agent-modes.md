---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づいてフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初の小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は既存の組織の開発フローに沿って、適切なものを適切な方法で作成できるようにします。このレッスンから、Issue に基づいて機能を作成し、コードを検証して動作を確認し、最終的にプロジェクトにマージする、エージェントを使った一般的な開発プロセスを学びます。

> [!NOTE]
> 機能のワークフローを続ける間、同じ会話とブランチを使います。通常はファイル種別ごとに異なるブランチや PR を使いますが、このワークショップでは中核となる概念に集中できるよう手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

機能開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリーとパブリッシャーで絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリーを組み合わせる方法など、コーディング前に合意が必要な点があります。Plan モードで決定してから、範囲を明確にした実装を Autopilot に依頼します。

## 背景

AI コーディングエージェントを開発フローに加えても、基本は変わりません。むしろ、重要性が増します。多くの開発者は次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 作成する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な進め方はチームや組織によって異なりますが、ほとんどは上記の流れを基にしています。

この標準的な方法に従うと、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証プロセスを経ることを確認できます。

## 会話モード

**会話モード**は、エージェントがどの程度自律的に動作するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive:** ユーザーとエージェントが一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan:** エージェントは最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot:** エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace とターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は、再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトに `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、機能を実装したりタスクを Copilot に任せたりする前に、アプローチを定義できます。複雑なタスクでは、時間をかけて計画することをお勧めします。Plan モードに切り替えて Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な方法を決めて計画を作成します。

3. フィルター機能の動作について質問されたら、希望に沿って回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 自分の決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot モードで Copilot に作業を任せます。

Autopilot では、Copilot が問題を完了したと判断するまで反復して作業します。

1. **Accept plan and build on autopilot (recommended)** または、インストール済みバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下のモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に従って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、実装がすぐ始まることがあります。先に計画を確認してください。依存関係の不足やポートの競合を Copilot が報告した場合は、チェックが完了したとみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージする前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルター機能とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードがよさそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces が転送するポートからサイトを開きます。

1. Copilot にアプリを起動するよう依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリーとパブリッシャーによるフィルター、および計画で合意した組み合わせを試します。
4. リセットと結果なしの場合の動作が、Issue と決定に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示][next-lesson]を使い、コードが文書化されたプラクティスに従って生成されるようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
