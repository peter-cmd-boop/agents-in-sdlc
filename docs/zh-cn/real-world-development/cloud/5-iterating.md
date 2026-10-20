---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

在整个实验中，你已与 GitHub Copilot 合作完成多项任务，以改进用户体验并增加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在来查看代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用审查功能之前，请先审查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此系统设置了一些安全限制：

- Copilot 对存储库只有读取权限，只能向用于编写代码的分支写入。
- 云智能体在 GitHub Actions 中运行，并会创建单独的临时环境来处理工作。
- 运行 GitHub Actions 工作流前，必须由人工审批。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，其中包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含为代码添加的文档。请使用 GitHub.com 中的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 访问外部资源（包括外部 MCP 服务器）的权限有限。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更详细描述的新拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息一下或回顾目前学到的内容。

4. 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些要求来自你在练习 2 开始时查看的自定义指令文件。

6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待审批的工作流提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 PR 检查部分中排队运行。如果一切正常，单个 Astro 应用的项目检查应会通过。完成可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像联系团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话进行处理。由于结果具有非确定性，无法提供固定措辞。你可以要求 Copilot：

- 在每个代码文件开头添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建一个介绍 Astro 应用结构的 README。

1. 添加评论，要求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用以上任一建议，也可以提出自己希望在代码库文档中看到的其他更改。
2. 选择 **View Session**，查看 Copilot 执行任务。Copilot 会启动新会话来完成这些更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。准备好后，将 PR 从草稿状态改为可审查状态，然后合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待审批的工作流提示。
7. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在 PR 检查部分中排队运行。如果一切正常，单个 Astro 应用的项目检查应会通过。完成可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，可以迭代改进结果，例如要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此议题时，你还没有更新自定义指令）。只需在 **Conversation** 选项卡中添加新评论，Copilot 就会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中通过 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件
   - 使用本地存储持久化用户偏好设置的方式
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码
6. 确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待审批的工作流提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 PR 检查部分中排队运行。如果一切正常，单个 Astro 应用的项目检查应会通过。完成可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换功能，确认它们能正常运行，并在页面重新加载后保留设置。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践。如果发现任何无障碍问题或改进空间，可以像之前一样在评论中标记 **@copilot**，要求它进行更新。

## 可选练习——继续委派任务

云智能体最适合处理真实待办事项，让你可以专注于其他工作。可以针对自己的存储库创建更多议题并分配给 Copilot，以培养这种工作习惯。可以尝试以下任务：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体路径。在这些课程中，你：

- **查看了存储库附带的自定义指令文件**，并在后续观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划任务并异步执行工作。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过标记 `@copilot` 请求修改，并批准工作流。

## 回顾与后续步骤

你已完成云智能体路径。如要继续探索，其他路径可以补充本次练习的内容：

- 🖥️ **[VS Code 路径](../../vscode/)**——直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 路径](../../cli/)**——在终端中交付两项经审查的更改，学习计划和自动驾驶模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接功能。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
