---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、それらの機能を明確に定義された QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタムエージェントに QA プロセスを調整させ、再利用可能なレポートを作成させます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。目的、指示、利用可能なツールを指定します。このワークショップでは `.github/agents/qa.agent.md` で QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズは、それぞれ異なる役割を持ちます。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し使うチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。ほかの機能を置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用できる QA プロファイルの作成を Copilot に依頼します。どのチェックを行い、どの範囲を守るかを定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA のワークフローと制約が Copilot に正しく伝わっていることを確認します。未完成または範囲が広すぎるエージェントが、確認だけのつもりで機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、表示名として指定すると分かりやすくなります。
3. プロファイルの指示を読み、要件を確認し、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更のコミットや PR の作成を行わないことを確認します。
5. 必要な役割や制約が抜けている場合は、既定のエージェントに修正を依頼してから続けます。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA エージェントを選択します。Issue と計画で決めた内容がすでにコンテキストに含まれています。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は利用可能なエージェントの一覧に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトを使って機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な QA プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらを機能の PR にまとめて [Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
