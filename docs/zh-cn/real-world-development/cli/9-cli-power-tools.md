---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派给云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多用于交互的斜杠命令。这些命令公开了高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已经完成。接下来了解其他功能：管理上下文、切换模型、共享会话，以及选择性地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大型或较复杂的任务时，可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在必要时自动压缩对话，也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及系统指令、工具、消息和可用空间之间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本身较短，变化可能并不明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。如果希望自行选择压缩时机，可以使用 `/compact`。切换到无关任务并想开始全新对话时，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发人员也有不同偏好。Copilot CLI 可以列出可用模型，并选择希望使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并共享经验可以帮助每个人改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出的内容发送给他人或发布前，请先审查。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要公开团队不打算分享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊到此结束。Copilot CLI 还提供了适用于大型任务的命令：

- `/fleet` 可以将彼此独立的子任务分配给子智能体并行处理。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给异步工作的 Copilot 云智能体；它可能会创建拉取请求。

这些命令是可选功能，因为它们可能会创建其他工作树或启动远程工作。尝试之前，请先创建新的、范围明确的任务，并通过正常工作流审查结果。如果要深入了解异步智能体工作方式，可以继续学习[云智能体工作坊][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 斜杠命令，可以配置工具、共享会话并查看幕后运行情况。本课中你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令可供探索。接下来，[回顾已学内容][next-lesson]并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
