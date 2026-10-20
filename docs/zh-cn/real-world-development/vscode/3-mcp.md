---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

刚刚使用代理模式构建了筛选功能。创建拉取请求之前，需要确认该功能在浏览器中确实可用。你将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR，而无需手动浏览应用。

本练习将：

- 了解 Model Context Protocol (MCP) 及 MCP 服务器如何为 Copilot 添加工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

代理模式可以访问编辑器以外的资源，因此功能更加强大。Copilot 通过 Model Context Protocol (MCP) 访问外部工具。MCP 是智能体与外部工具和服务通信的标准方式。

![代理模式内部工作原理示意图：展示代理模式如何与上下文、大语言模型和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问外部工具和服务，以获取最新信息（使用资源）并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理二者之间的通信，例如连接现有 API 或 NPM 包等本地工具。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用它创建和更新存储库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化功能。AI 智能体可以使用它浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器可供使用，支持访问不同的工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与 MCP 生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 其中已经配置了 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 配置由项目模板提供，因此上一练习中 Copilot 能够读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装。VS Code 通过 HTTP 连接到服务器，Copilot 首次使用其工具时需要使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的浏览器，正好可以用来测试刚完成的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列，最终文件如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器。codespace 没有可用于显示浏览器的桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要工具时启动服务器，并提示你确认信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]让 Copilot 可以操作真实浏览器。无需手动选择应用中的控件来测试功能，智能体可以打开页面、浏览页面、应用筛选条件并读取结果，然后向你总结观察结果。这样可以在不离开对话的情况下快速确认功能符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而非屏幕截图。因此智能体会根据结构化的标记元素（例如按钮、链接和列表项）进行推理，这与辅助技术的工作方式类似。快速功能检查也能顺便完成简单的无障碍检查。

连接服务器并启动应用后，向 Copilot 发送以下提示，让它测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行每个步骤，然后报告结果。请将报告与议题中的验收标准进行比较。如果发现问题，可以提出后续问题，或要求 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 上运行。如果已停止开发服务器，请先重新启动，再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器。如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示已更改文件的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括在 github.com 上操作或使用 GitHub 命令行界面 (CLI)。既然已经在使用 GitHub Copilot，就让它来创建 PR 吧。它可以查找相关议题，并创建与其关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认已在智能体下拉列表中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证，请按照提示授权。
5. 查看 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接以审查拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中你：

- 了解了 Model Context Protocol (MCP) 及 MCP 服务器如何为 Copilot 添加工具；
- 将 Playwright MCP 服务器添加到工作区；
- 使用它在浏览器中手动测试筛选功能；
- 发布分支，并为筛选工作创建拉取请求。

你已经使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等聚焦任务。

## 资源

- [什么是 MCP？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
