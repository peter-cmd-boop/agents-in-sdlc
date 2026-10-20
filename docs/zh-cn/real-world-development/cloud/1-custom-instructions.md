---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保这些上下文能够被访问。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 了解如何通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条新的全局标准。

> [!NOTE]
> 与 VS Code 和 CLI 路径不同，此处不会使用前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后在本路径中审查云智能体创建的拉取请求时，可以看到指令文件的影响。

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

## 添加新的存储库标准

接下来需要编辑一处内容：添加一条全项目规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本路径中分配议题时，云智能体会采用这些指导。

编辑前，请先创建并切换到新分支（云智能体会读取议题目标分支上的指令，但仍应规范提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，指令更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，在下一练习中分配议题时，云智能体就能读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本工作坊，可以跳过分支步骤，直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何使用此项目的指令文件，并向 `.github/copilot-instructions.md` 添加了一条存储库级标准。接下来几课中，云智能体创建的拉取请求会展示这条标准的作用。

下一步，将[首个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
