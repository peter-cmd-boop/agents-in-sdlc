---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，整合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将：

- 了解自定义智能体如何使用指令、技能和 MCP 工具。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 之前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程并提供可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的专用 Copilot 版本。配置文件会说明智能体的目的、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

此前使用的自定义功能各有用途：存储库指令描述团队标准；`quality-checks` 技能封装可重复的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何运用这些功能评估需求并报告结果。它不会取代这些功能，也不需要另开会话。

## 创建 QA 配置文件

创建功能 PR 之前，让 Copilot 创建可重复使用的 QA 配置文件。该配置文件将定义 QA 执行的检查及其工作边界。

1. 返回 Codespace，并确认筛选功能的对话处于 Interactive 模式。
2. 让默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查它的配置文件，确认 Copilot 捕获了预期的 QA 流程和工作边界。这样可以避免你只想验证功能时，智能体执行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 是必需字段；`name` 为可选字段，但填写后可为智能体提供清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、在修改实现代码前询问，并且不会提交更改或打开拉取请求。
5. 如果配置文件缺少这些职责或边界，请在继续之前让默认智能体修改。

## 根据议题运行 QA

Copilot CLI 会在会话开始时加载项目智能体。创建配置文件后，继续使用同一个筛选功能对话，然后选择 QA，让它使用已有的议题和规划决策。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建智能体，它可能不会出现在可用智能体列表中。不过该智能体已经存在，可以使用上述命令启用。

2. 使用以下提示要求 QA 审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后，阅读它提供的报告。

## 总结与后续步骤

你已在工作流中添加可重复使用的专用角色，并审查其工作。本课中你：

- 了解了自定义智能体如何使用指令、技能和 MCP 工具。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其结果。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪，可以进行审查。接下来，将这些内容[整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
