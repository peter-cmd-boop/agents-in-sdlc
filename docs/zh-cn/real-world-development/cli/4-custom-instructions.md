---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 可以访问相关指导。[指令文件][instruction-files]不仅描述希望生成*什么*代码，也规定代码应*如何*组织。筛选功能已经完成，现在来探索 Copilot 使用的指令、添加一项文档标准，并将其应用于代码。

本课将：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 和其他优秀的软件开发团队一样，有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的设计决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括适用时可注入的 `db` 参数）。
- 可重复使用的 Astro 组件应说明其 `Props` 约定，相关代码更改时注释也应保持更新。
- 应保留现有格式和 lint 指导。

通过使用指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解代码风格和要求。例如可以指定首选编码约定、库和注释形式。指令既可以应用于整个存储库，也可以针对特定文件类型或任务。

有两种指令文件：

- `.github/copilot-instructions.md`：单个指令文件，会发送给 Copilot 针对该存储库的**每个**请求，其中应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他指令格式的支持各不相同。依赖某种特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览项目中的自定义指令文件

起始项目已经包含了一组指令文件。先看看其中已有内容，再进行更改并了解效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而非终端中）打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简述和编码指导。这些指令适用于此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 指定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分为更小、可重复使用的文件，并为 Copilot 提供参考示例。

## 更新指令以符合团队规范

现有文件已具备良好基础，但仍有不足。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码中包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，应位于第 35 行附近。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。更新指令后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认检出的分支为 `game-filters-cli`。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明代码内容。

## 总结与后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能。本课中你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，[自定义并运行可重复使用的 quality-checks 技能][next-lesson]，确保一致地执行 lint 和测试。

## 资源

- [向 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
