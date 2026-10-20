---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、事前登録されたフィルター機能の Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) 用のカスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証を評価しました。
- フィルター機能全体をレビューし、Agent Merge にフィルター機能の PR をマージする権限を与えました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップには、更新した `main` から作成した別々のブランチで進める2つの PR があります。

1. **星評価:** 既存の `starRating` と、未評価の場合の表示をゲームカードに追加します。
2. **フィルター機能と品質のワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を1つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、ブラウザーでの手動確認、MCP を介した Copilot のブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは、要件とテストカバレッジのレビューにその結果を加え、PR 作成前にまとめて確認できるようにしました。

追加するテストは、実際の不足を補う必要があります。QA を実行した結果、追加のテストが不要でも問題ありません。変更後はコードと根拠を確認し、影響を受けた検証を再度実行してください。

## ベストプラクティス

Copilot に渡すコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し行う作業を説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの概要だけで判断せず、実際の変更とツールの実行結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討、**Interactive** は小さな変更を確認しながら進める作業、**Autopilot** は範囲を明確にしたタスクに適しています。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。何を作成するのか、なぜ必要なのか、どのように作るのかを明確に伝えると、結果が大きく変わります。

## さらに試す

中核のワークフローを学びました。次の CLI 機能も試してみましょう。

- `/review` を使ってコードレビューエージェントに変更を分析させます。
- `/rubber-duck` を使って問題を説明し、別の視点を得ます。
- `/fleet` を使って独立したサブタスクを並列に実行します。
- `/worktree` を使って別のタスクを分離します。
- `/delegate` を使って Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。製品コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどに活用してください。学んだことをチームと共有し、ほかの人からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、[Foundry Backer Concierge の省略可能なシリーズ][foundry]に進みます。異なる環境での体験を比べる場合は、[VS Code のワークショップ][vscode]、[GitHub Copilot app のワークショップ][app]、または [Copilot cloud agent のワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
