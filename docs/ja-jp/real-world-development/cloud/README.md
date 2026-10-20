---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot をクラウド上で非同期に動作させることができます。GitHub 上で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査して変更を加え、プルリクエストを作成します。その間、ほかの作業を続けられます。

演習では、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて作業を実装させます。カスタムエージェントを確認して作業方法を調整し、エージェントのダッシュボードからセッションの進捗を監視して指示を出します。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud agent][ex2] | 非同期エージェント | Issue を GitHub Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使います |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視し、方向付けます |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次のステップを選びます |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free では利用できません。Copilot Business または Enterprise の場合、組織の管理者が有効にする必要があります。

> [!NOTE]
> このワークショップでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ワークショップ][cli]または [VS Code ワークショップ][vscode]を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
