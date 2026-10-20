---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义智能体][previous-lesson] |
|:--|

在前几项练习中，你要求 Copilot 云智能体完成了三项任务，以改进用户体验并增加功能。云智能体虽然会异步自主运行，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有有未关闭拉取请求的智能体任务、了解已执行的操作，还可以引导正在运行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求它添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队发现也应该添加浅色模式。既然正在更新网站样式并添加切换功能，一并实现浅色模式就很合适。现在需要引导智能体，让它同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态：

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 为代码库更新文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前工作状态和采用的方法而异。

4. 同时查看右侧的拉取请求 (PR) 面板。你可以在此查看 PR 和已更改文件，以便进一步监控。

## 引导云智能体

现在已经了解正在运行的任务，接下来请在 Copilot 实现高对比度模式时，让它也添加浅色模式切换。

1. 选择与添加高对比度模式相关的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始执行任务。看到类似以下的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求纳入工作流程。

这同样需要几分钟，可以趁此机会回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过此控制中心跟踪所有正在运行和已完成的任务、审查正在执行的工作，还可以调整正在运行的任务。

本课介绍了以下概念：

- 使用 Mission Control 和智能体页面监控云智能体任务。
- 引导正在运行的会话，要求它添加其他功能。

Copilot 正在完成无障碍功能。接下来，学习[如何迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
