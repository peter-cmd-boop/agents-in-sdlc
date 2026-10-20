---
title: "课程 10 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可重复使用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中制定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能运行结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 你交付的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建单独的分支：

1. **星级评分**：在游戏卡片上显示已有的 `starRating`，并明确显示未评分状态。
2. **筛选与质量工作流**：实现筛选功能、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你都使用同一个对话和分支。为了简化工作坊，这些工作全部合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、在浏览器中自行检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按照新格式报告结果。QA 汇总这些结果，并在创建 PR 前审查需求和测试覆盖范围。

新增测试应该弥补真实缺口；如果 QA 运行后认为不需要增加测试，也可能是正确结果。授权合并前，请审查代码和验证证据。代码更改后，请更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同会话中重复使用这些自定义设置，并根据团队需要调整。指令规定标准，技能说明可重复的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前规划方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可以选择速度更快的模型，复杂任务则可以选择能力更强的模型。

上下文和基础设施同样重要。清楚说明要构建的内容、原因和方式，会显著影响结果。

## 深入探索

核心工作流已经完成。还可以了解一些其他 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一个角度。
- 使用 `/fleet` 并行协调彼此独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方式就是继续使用它！可以将它用于生产代码、个人项目，或一直想做却尚未开始的小应用。与团队分享经验并互相学习，同时继续探索文档。

若要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若想比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
