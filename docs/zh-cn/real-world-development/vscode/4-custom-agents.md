---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是专门针对开发工作流中某类任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，从而更有效地执行特定类型的工作。团队可以将专业知识整理为可重复使用的智能体，例如强制遵循 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含至少带有 `name` 和 `description` 字段的 YAML frontmatter，后面是用于定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]之间存在一些重叠。两者主要通过 Markdown 文件定义，并指导 AI 执行操作。最简单的区分方式是：**自定义智能体是执行者，技能是工具**。

自定义智能体拥有自己的上下文窗口，负责协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与其他 AI 用法一样，应不断测试和迭代，找到适合环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，当前深色主题的文本与背景颜色对比不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户启用或关闭的高对比度模式。

无障碍至关重要，因此需要尽快实现此功能。接下来将使用自定义智能体生成该功能。

## 查看无障碍自定义智能体

已经为无障碍工作创建了自定义智能体。查看其内容，了解它会如何引导 Copilot。

返回 codespace，然后打开终端，为无障碍工作从 `main` 创建一个新分支（这样可将筛选 PR 与练习 3 的更改分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` 是自定义智能体必需的 frontmatter 字段。

3. 浏览后续部分，了解其中列出的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话会使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异，每个步骤会在需要时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认情况下显示当前活动智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中以内联差异形式显示文件路径和更改。
6. 随着编辑逐渐生成，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会对存储库进行实际更改，例如更新 Astro 组件、CSS 和相关测试。

下一课将审查并引导这些正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义智能体，可以将团队的专业知识和标准整理为可重复使用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，审查工作进度，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
