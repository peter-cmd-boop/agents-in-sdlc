---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを調べ、レポート形式をカスタマイズし、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも多くの作業があります。コードを手動で確認し、指示ファイルを使って標準に従わせました。次に、テストや lint、継続的インテグレーション (CI) のほかの部分を扱います。

こうした作業には、**エージェントスキル**が適しています。Copilot がこれらの操作を正しく実行できるようにします。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用できるタスクの指示、実行可能なスクリプト、補助リソースをまとめて、必要に応じてエージェントが読み込みます。基本的にはスキル名のフォルダーと、その中の `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキルの名前と説明を定義するフロントマター、機能の概要、呼び出す場面の指針を記載します。フォルダーには、スキルが使うスクリプトやほかのリソースを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、プロジェクト内の `.github/skills` フォルダーに置いてチームで共有、再利用することも、ユーザーのスキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出す場面を判断するのに役立ちます。
3. 指示を読み、テストと lint のプロセスをどのようにガイドするか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、コマンドまたは自然言語でスキルを直接呼び出せます。3種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行し、成功したか、ツールがどのような結果を報告したかを示すレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。変更を試してみましょう。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 最初とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能の PR に含めます。次は [Playwright MCP サーバー][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## スキルの例

以下は参考例であり、追加の作業ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
