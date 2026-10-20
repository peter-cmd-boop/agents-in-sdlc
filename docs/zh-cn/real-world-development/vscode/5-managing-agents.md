---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

Copilot 处于代理模式时，会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时在本地计算机上执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审查每项差异、接受或拒绝单项更改，还可以通过后续提示引导对话，在不离开编辑器的情况下改进或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，要求它在高对比度模式之外添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外要求：他们还希望为觉得默认主题太暗的用户提供浅色模式切换。无需从头开始新对话，只要引导现有会话继续处理该功能即可。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作期间，在聊天中滚动查看各项工具调用。你会看到文件读取、文件编辑和它请求运行的终端命令。
3. 智能体提出文件编辑后，差异会以内联形式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看此会话期间智能体修改的完整文件集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。它应该会显示一条总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，可以在同一对话中扩展功能，添加浅色模式切换。继续使用当前聊天会话，智能体会保留之前工作的上下文，包括它修改的组件、采用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查已有更改，并提出添加浅色模式的修改。
3. 像审查高对比度模式一样审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如尝试重构切换 UI），可以使用聊天输入框旁边的 **Stop** 按钮停止它，然后发送说明性的后续消息。

> [!TIP]
> 使用后续消息引导正在运行的会话，比从头开始更快。可以要求它调整方案（“简化 CSS，使用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，而是将浅色模式集成到现有设置面板中”）或扩展范围（“为两个切换开关都添加键盘快捷键”）。

## 审查工作文件集

提交工作前，快速审查智能体修改的所有文件：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认实现遵循无障碍原则，包括 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储持久化设置。
4. 确认更改没有问题后，在 Source Control 面板中暂存并提交。下一课将发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 代理模式监控并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并发送后续提示来扩展工作，同时保留上下文。

本课介绍了以下概念：

- 实时监控智能体工作。
- 接受和拒绝单项编辑。
- 在会话进行期间使用后续提示调整工作方向。

在[下一课][next-lesson]中，你将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体所做的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
