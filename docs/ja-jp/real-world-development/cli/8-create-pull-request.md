---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキル、QA プロファイル、テストをレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1つのブランチに保存されています。全体をレビューしてプルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質改善の作業を1つの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージライフサイクルの自動化について学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業を通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、開発者がマージの承認権限を保ちながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている項目に対応します。たとえば、失敗した継続的インテグレーション (CI) チェックを修正し、レビューコメントに対応し、必要に応じてリベースします。その後、リポジトリで利用できる場合は GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せられますが、有効にする前に変更をレビューし、マージできる状態だと判断してください。

## Agent Merge で PR を管理する

コードが揃ったので、全体をレビューして PR を作成し、Agent Merge に残りの作業を任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功したらマージします。
7. Copilot が `PR #14 was squash-merged successfully.` のようなメッセージを表示することを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限の不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されているときに、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発作業の一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
