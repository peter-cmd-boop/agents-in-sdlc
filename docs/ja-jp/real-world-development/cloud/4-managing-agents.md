---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習で、ユーザー体験の向上と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクの管理には、GitHub.com の[エージェントページ][agents-page]など、複数のツールを使えます。この画面では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼して、作業の方向を調整します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームは同時にライトモードも追加したいと考えました。サイトのスタイルを更新して切り替え機能を追加する作業をすでに進めているため、ライトモードも含めるのが適切です。ハイコントラストとライトモードの両方を追加するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域にあるタスクの一覧を確認します。次のようなタスクがあるはずです。
   - コードのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを1つ選択し、Copilot が実行した操作を確認します。次のような操作が表示されます。
   - リポジトリのチェックアウト。
   - Copilot 用の環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクの完了に向けた作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側のプルリクエスト (PR) 領域も確認します。ここから、PR と変更されたファイルを監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して実装が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中のセッションに指示を出すダイアログを表示](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に加えることを確認します。

## Copilot に作業を続けさせる

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が終わると、新しい依頼を作業に取り入れます。

今回も完了まで数分かかります。その間に、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視してガイドする中心的な画面である Copilot agents ページを確認しました。ここでは、実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクの範囲や指示を変更できます。

次のことを確認しました。

- mission control とエージェントページで cloud agent のタスクを監視する。
- 進行中のセッションに指示を出して機能を追加する。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
