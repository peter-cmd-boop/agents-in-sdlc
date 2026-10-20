---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要查找所有相关文件、进行修改、运行测试、调试、找出遗漏的文件、再次更新……这样的过程似乎没有尽头。

Copilot 代理模式正是为此而设计的。

代理模式可以在 IDE 中更自主地工作。它会像开发人员一样，先探索项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。下面来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但代理模式和云智能体针对两种不同的体验。代理模式在 IDE 中执行任务，可让你快速获得反馈并与其交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 在将 Copilot 生成的更改合并到代码库之前，如何审查更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发行商筛选。上一练习中已经添加了发行商辅助函数，现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改之前，先浏览 Tailspin Toys 网站，了解当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，允许用户列出游戏并查看详情。它是一个在构建时将页面渲染为静态 HTML 的单一 Astro 应用。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，无需单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用小型、作用域明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。完成上一练习后，你应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的 URL 提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示你在浏览器中打开 `http://localhost:4321` 的对话框，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的专用隧道，网站现在已在其中运行。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，其中包含标题、图片和说明。
- **游戏详情页**：选择游戏后进入详情页，查看标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异，每个步骤会在需要时说明。

网站的初始版本已能正常运行，但还需要添加新功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；现在让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体能够自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非在推荐某个特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此示例中采用此模型，希望代码建议相对一致，确保学习体验良好。不过，大语言模型具有概率性，生成的建议可能与示例不同。这完全正常，也符合预期。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问存储库的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 会根据它认为对用户最有帮助的程度为议题排序。

## 查看指令文件

开始生成代码前，先查看用于向 Copilot 提供工作上下文的指令文件。接下来将使用[用户界面 (UI)][ui-instructions]文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型具体指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以重复使用内容、拆分较大的指令集，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用进行以下三项更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，测试必须运行并通过，才能将更改合并到代码库。Copilot 代理模式可以完成这些工作。现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题要求 Copilot 实现功能，并发送以下提示：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件。它应该找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此无需在工作进行期间选择它们。

7. 按 Copilot 提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中提示用户确认是否运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 工作期间可能会暂停一段时间，甚至出现一些测试失败。这没关系。Copilot 会反复生成代码并运行测试，直到完成任务且未检测到错误。

   ![包含 Copilot 代理模式完整会话的 Chat 屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解到：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- 如何在合并代码之前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为此更改创建拉取请求。

### 扩展练习：实现分页

随着游戏列表不断增长，需要提供分页功能。使用本练习中学到的技能，提示 Copilot 为网站添加分页。可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 决定分页的实现方式，例如允许用户选择每页大小，或使用固定值。
- 编写提示时提供足够的指导，让 Copilot 按照期望实现功能。
- 可能需要迭代并提供上下文，请 Copilot 修改代码。这是使用 Copilot 的常见流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：各自的功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
