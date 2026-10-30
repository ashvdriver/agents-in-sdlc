---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。打开拉取请求之前，应确认它在浏览器中确实可用。无需自己逐项点击操作，你可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器为你测试该功能，然后发布分支并打开 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 是什么，以及 MCP 服务器如何通过新工具扩展 Copilot，
- 将 Playwright MCP 服务器添加到工作区，
- 让 Copilot 使用它在浏览器中手动测试筛选功能，
- 发布分支并为筛选工作打开拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够触及编辑器之外的内容时，会强大得多。模型上下文协议 (MCP) 正是 Copilot 实现这一点的方式，它是代理与外部工具和服务通信的标准方式。

![展示代理模式内部工作原理，以及它如何与上下文、LLM 和工具交互的示意图，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时与外部工具和服务通信，从而获取最新信息（通过资源），并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问，MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。MCP 服务器负责管理 AI 代理与外部工具（如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器代表 AI 代理可以访问的一组不同的工具和资源。

以下是几个常用的现有 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：该服务器提供一组用于管理 GitHub 存储库的 API 访问，允许 AI 代理执行创建新存储库、更新现有存储库，以及管理议题和拉取请求等操作。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：该服务器使用 Playwright 提供浏览器自动化功能，允许 AI 代理执行导航到网页、填写表单和点击按钮等操作。

还有许多其他 MCP 服务器，可访问不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，以提升生态系统的可发现性并促进贡献。

> [!CAUTION]
> 在安全方面，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用 MCP 服务器之前，请仔细查看其源代码、核实发布者，并考虑其安全影响。只使用你信任的 MCP 服务器，并谨慎授予对敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应能看到已为你配置好的 `github` 服务器：

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

该 `github` 条目随项目模板提供，因此 Copilot 在上一练习中能够读取你的议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，因此无需在本地安装任何内容：VS Code 通过 HTTP 连接到它，你在 Copilot 首次使用其中某个工具时使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了一个可控制的浏览器，这正是测试功能所需要的。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件如下所示：

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

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 通过运行 `npx @playwright/mcp@latest` 在你的机器上启动它。`--headless` 标志指示 Playwright 在没有可见窗口的情况下运行浏览器，这在没有桌面可显示的 codespace 中是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已安装。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code **按需**启动 MCP 服务器：Copilot 首次需要某个服务器的工具时，VS Code 会启动该服务器，并要求你确认信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它可立即启动服务器，并确认配置正确。
2. 如果 VS Code 要求你确认信任该服务器，请查看配置并选择信任，以便服务器能够启动。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可驱动的真实浏览器。无需你逐项点击应用来检查工作成果，代理可以打开页面、导航、应用筛选并将结果反馈给你，然后总结它所看到的内容。这是在不离开对话的情况下，确认功能符合预期的最快方式。

在底层，Playwright MCP 服务器基于页面的[无障碍树][playwright-mcp-server]而非屏幕截图工作。这意味着代理会像辅助技术一样，针对结构化、带标签的元素（按钮、链接、列表项）进行推理，因此一次快速的功能检查同时也是一次简易的无障碍合理性检查。

服务器连接且应用运行后，让 Copilot 测试你刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行并报告发现。请对照议题中的验收标准阅读它的总结。如果发现异常，可以追问，或在打开拉取请求之前让它回头修复代码。

> [!NOTE]
> 此测试要求应用运行在 `http://localhost:4321`。如果已停止开发服务器，请在发送提示词之前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器。如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以打开拉取请求 (PR)，供团队审查。第一步是发布 `filtering-vscode` 分支。

1. 导航到 codespace 中的 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示所做更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括通过 github.com 和 GitHub 命令行界面 (CLI)。但既然已在使用 GitHub Copilot，不妨让它为你创建 PR。它可以找到相关议题，并创建与之关联的 PR。

1. 导航到 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确保代理下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和执行操作所需的任务。首次使用 GitHub 工具时，可能会提示你使用 GitHub 进行身份验证，请按提示允许。
5. 留意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**请先不要合并**。

## 总结与后续步骤

恭喜！在本练习中，你：

- 了解了模型上下文协议 (MCP) 是什么，以及 MCP 服务器如何通过新工具扩展 Copilot，
- 将 Playwright MCP 服务器添加到工作区，
- 在发布之前，使用它在浏览器中手动测试了筛选功能，
- 发布了分支，并为筛选工作打开了拉取请求。

你使用 MCP 服务器*测试*了一项功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 到底是什么？为什么人人都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
