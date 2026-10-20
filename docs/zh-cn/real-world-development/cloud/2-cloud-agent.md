---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题。这可能是尚未解决的安全问题、需要更新的旧代码，也可能是长期搁置、一直没有时间实现的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成工作后，它会创建一个供开发人员审查的草稿 PR。这样可以将繁琐工作交给智能体，让开发人员专注于更重要的工作，加快开发进度。

本课将探索以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要解决。最初负责创建网站的承包商没有妥善编写文档，事实上，网站完全缺少文档。团队首先希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望每个游戏详情页显示相关游戏，也就是同一类别中的其他游戏。暂时不需要精致的实现，只需提供可用于验收测试用户体验的版本。目前这项工作有所延误，不过还有优先级更高的议题。

这类工作很容易被一再推迟，因此非常适合分配给 Copilot 云智能体。云智能体可以在后台异步处理，让开发人员继续其他工作，然后回来审查 Copilot 的结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与开发人员协作时类似，可以通过多种方式分配任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或完成后查看 Copilot 的工作。建议方案准备好供你审查时，Copilot 会在拉取请求中通知你。

## 明确界定指令范围的重要性

GitHub Copilot 并没有什么魔法，不能代替思考。即使看似简单的操作，深入分析后也可能发现复杂之处，因此[请谨慎界定云智能体任务的范围][cloud-agent-best-practices]。应将它视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的指导作用。如果你已完成自定义指令练习，就已经看到 `.github/copilot-instructions.md` 的效果；如果尚未完成，现在可以快速浏览一下。在将任务分配给 Copilot 云智能体之前，请先只读查看此存储库中已有的指令文件，稍后便能识别它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将此指令限定到 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意由这些指令文件带来的 TSDoc 文档注释、TypeScript 规范和文件注释头。稍后的练习还会在审查 PR 时再次介绍这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要配置环境并运行设置脚本，确保各项内容处于正常状态。将任务分配给 Copilot 时也一样，它会以类似软件工程师 (SWE) 的方式工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置，让它在智能体开始工作前运行，以便安装所需的开发工具和依赖项。本实验已预先配置此工作流，以便顺利完成练习。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器，以及单个 Astro 应用使用的迁移和填充数据后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 权限应仅限于这些步骤所需的最低范围。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要授予 `contents: read` 权限。如果不在设置步骤中克隆存储库，Copilot 会在步骤完成后自动执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几个关键点：

- 其中包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。在等待 Copilot 触发工作流之前，可以用它测试工作流是否正常。

## 添加文档

大家都知道文档很重要，但许多项目的文档已经过时，或者根本没有文档。这类技术债务往往一直无人处理，降低工作效率，也增加维护代码库和引入新开发人员的难度。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档。稍后的练习中将回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等一两分钟并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它需要创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 准备执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括如何探索代码、了解当前状态、暂停思考并确定方案，以及如何创建代码。

此过程可能需要几分钟。Copilot 云智能体的主要优势之一就是可以异步执行任务，让你专注于其他工作。我们将利用这一点，再分配一项任务给 Copilot 云智能体，然后转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作，让你和智能体分别专注于不同任务。为设计团队添加相关游戏区域也许不需要很长时间，但这段时间仍可用于处理其他工作。现在就将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会看到议题的首条评论中出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项任务！Copilot 云智能体的工作方式类似软件工程师，因此无需一直监控；任务完成后再回来审查即可。接下来，创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot 异步处理。你可以用它解决技术债务、开发新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作。现在可以继续学习如何创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并支持前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
