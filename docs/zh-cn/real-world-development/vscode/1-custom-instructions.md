---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保这些上下文能够被访问。[指令文件][instruction-files]可提供相关指导，让 Copilot 了解代码生成的方式。

本练习将：

- 了解如何通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在现有指令下生成第一个筛选数据片段（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的全局标准；
- 使用相同提示再次生成代码，观察代码如何遵循新增标准；
- 提交指令更新和筛选基础代码到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不一致。Copilot 的输出具有非确定性。本练习旨在观察更新指令后代码行为的*趋势*，并不要求输出与示例逐字相同。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的软件开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码中的文档应使用 TSDoc 文档注释。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解代码风格和要求。你可以借此引导 Copilot 提供更相关的建议和代码片段，例如指定编码规范、库，以及希望添加的注释类型。指令既可以应用于整个存储库，也可以针对特定文件类型或任务。

有两种指令文件：

- `.github/copilot-instructions.md`：单个指令文件，会发送给 Copilot 针对该存储库的**每个**请求。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的指令文件，例如 TypeScript 指南，或创建 UI 组件、单元测试的要求。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了一种具有代表性的方法。总体而言：

- 在 `copilot-instructions.md` 中重点提供项目级指导，例如项目用途、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，让指导清晰易懂，并提供代码示例说明应采用和避免的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过尝试，你会逐步找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的指令文件。浏览此项目中的文件时，你会发现它们涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品中的操作方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。与当前产品相关的课程会在需要时介绍这些操作。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简述以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些指导适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来确定适用文件。在此处，匹配 `**/*.test.ts` 的任何 TypeScript 测试文件都会应用此指令。
5. 留意专门用于创建单元测试的指引。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动至底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并提供参考示例。（这些路径相对于指令文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码内文档。接下来的步骤将添加 TSDoc 文档注释和文件注释头的规则。

## 创建分支

按照最佳实践，为这些更改创建分支：

1. 返回上一练习中打开的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异，每个步骤会在需要时说明。

为了解自定义指令的作用，先在当前指令文件状态下发送提示。稍后更新指令后，再发送同一提示以比较差异。

1. 关闭之前练习打开的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常包括辅助函数文件及其测试。
6. 留意建议的辅助函数是一个类型化函数，它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这是 `.github/instructions/drizzle.instructions.md` 中数据层约定的作用（该文件适用于 `src/lib/*.ts`）。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，即使没有明确要求，它也可能添加文档注释。这没有问题；本练习重点是更新指令后行为的一致性。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。让我们添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。查看此处记录的项目编码标准；其中尚无代码内文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别使用 `.github/copilot-instructions.md` 创建全局指令，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务创建指令。存储库级文件适合记录项目范围的标准，例如刚添加的文档注释规则。

## 再次发送提示并查看变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供用于检索数据库中发行商信息的函数。
    */
   ```

5. 留意建议的函数现在带有类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的列表，包含其 id 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

现在已经成功引导 Copilot 遵循新标准，并将其应用于真实代码。下一课将在此基础上继续构建。

## 提交指令并推送分支

指令文件与其他项目资源一样，也由源代码管理。请提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础代码更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，应创建一个拉取请求，并关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理。提示 Copilot 查找议题、创建 PR 以关闭议题，然后合并。

> [!NOTE]
> 这是本工作坊首次让 Copilot 代表你操作 GitHub。项目模板已预先配置工作区所需的 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习将介绍此连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新建的发行商辅助函数基础代码：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先检查命令，并在合适时选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求执行 GitHub 操作和 shell 命令时，请先检查命令，并在合适时选择 **Allow**。

现在已经借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何使用此项目的指令文件，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次发送相同提示，观察建议的代码如何遵循新标准；
- 将指令更新和辅助函数基础代码提交并合并到 `main`，供下一课使用。

接下来，使用[代理模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
