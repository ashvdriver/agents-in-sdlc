---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们需要处理数据、外部服务，甚至让 Copilot 使用更多自动化功能。这正是 MCP 服务器的用武之地。MCP 服务器可让 Copilot 超越 CLI 的内置功能，获得更多工具和服务。

本课将介绍如何：

- 了解模型上下文协议 (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 在尚未提供 Playwright MCP 服务器时添加它。
- 让智能体驱动浏览器，探索你的筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新需要真正与 UI 交互。你希望让 Copilot 像用户一样使用正在开发的网站，进一步自动化更改流程，让你更有把握确认更新符合预期。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而获取最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问，MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器代表 AI 智能体可以访问的一组不同的工具和资源。

以下是几个常用的现有 MCP 服务器：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API 访问。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，以提升生态系统的可发现性并促进贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，请查看其源代码、核实发布者，并考虑其安全影响。

## 添加 Playwright MCP 服务器

如果尚未提供 Playwright MCP 服务器，请添加它，让 Copilot 能够像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称处输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) 保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 中可用的 Playwright MCP 服务器来完成同样的验证。

1. 使用以下提示词，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 实际上不必告诉 Copilot 使用 MCP 服务器，它通常能自行判断。但既然你知道该用什么，主动指明方向也无妨，这有助于获得更一致的结果，并节省一些令牌。

2. 观察 Copilot 列出它在浏览器中为确认功能正常而执行的各个步骤。
3. 阅读报告，确认一切符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并向你提供报告。

## 总结与后续步骤

恭喜！你已在 Copilot CLI 中使用 Playwright MCP 服务器，在真实浏览器中探索了功能。回顾一下，你：

- 了解了模型上下文协议 (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 在尚未提供 Playwright MCP 服务器时添加了它。
- 让智能体驱动浏览器，探索了筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，让它在专家角色中整合技能和浏览器工具。

## 资源

- [MCP 到底是什么？为什么人人都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
