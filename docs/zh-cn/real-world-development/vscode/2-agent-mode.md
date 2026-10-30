---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行运行测试等操作。作为开发人员，你的工作流通常是：找出所有必要的文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……如此往复。

这正是 Copilot 代理模式的用武之地。

Copilot 代理模式旨在让 IDE 中的 Copilot 更自主地工作。它的行为类似于开发人员：先探索现有的项目结构，执行必要的更新，运行测试等任务，并自动修复发现的错误。下面来看看如何使用代理模式为站点引入新功能。

> [!NOTE]
> 虽然名称相近，但代理模式和云代理面向两种不同的体验。代理模式在 IDE 中执行任务，可实现快速的反馈循环和交互。云代理被设计为同行程序员，像团队成员一样异步工作，并通过议题和拉取请求与你互动。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并进行协调一致的更改。
- GitHub Copilot 代理模式如何在 UI 和数据层中实现新功能。
- 如何在合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发行商筛选。上一练习中你已添加了发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改之前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是一个面向开发者主题桌游的众筹平台，用户可以列出游戏并查看其详情。网站是单个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，所有交互功能都通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 添加。

### 启动网站

为便于运行网站，项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。完成上一练习后，应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)，在 Codespace 中打开新的终端窗口。
3. 为此次筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应会出现显示 URL 的横幅，类似于：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd> (Mac) 或 <kbd>Ctrl</kbd> (Windows/Linux)，选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 链接，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的专用隧道，该 codespace 现在正在托管你的 Web 服务器。

### 浏览网站

网站运行后，可以探索其功能。网站的主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择某款游戏后，会进入详情页，其中包含该游戏的更多信息，如标题、说明、发行商和类别。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习之前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。模式和模型的选择因练习而异，各步骤会在需要时说明。

网站的初始实现已可正常运行，但我们希望通过添加新功能来增强它。先查看待办事项。基于模板设置项目时，系统已自动为你创建了一组议题作为待办事项，让 GitHub Copilot 将这些条目展示给你。

1. 在聊天视图的代理下拉列表中选择 **Agent**。**Agent** 代理会自主规划并实施跨文件的更改、运行终端命令并调用工具。

   ![聊天视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 在可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者对任何模型都没有偏好。构建本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中采用了该模型。希望你收到的代码建议大致一致，以保证良好的体验。但由于 LLM 具有概率性，你可能会发现收到的建议与工作坊中所示不同，这是完全正常的。

> [!NOTE]
> 由于 LLM 的概率性，Copilot 可能使用不同的 MCP 命令，但应仍能完成任务。

3. 向 Copilot 发送以下提示词，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 甚至已根据它认为对用户最有用的程度，为各项排好了优先级。

## 查看指令文件

在启动代理生成代码之前，不妨先查看你将用来为 Copilot 的工作提供上下文的指令文件。你将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含如何着手为网站添加功能的上下文。

1. 在 codespace 中，导航到 `.github/instructions/ui.instructions.md`。
2. 留意关于如何着手添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍方面的原则。
   - 指向各类文件专用指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件既可以引用其他指令文件，也可以引用项目中的文件，路径相对于指令文件所在位置。这样可以复用内容、将复杂指令拆分为更易管理的小块，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项独立更新：

- 添加或完善数据层 (`src/lib/`) 中其余的筛选逻辑
- 添加或更新筛选行为的测试
- 更新游戏列表页面，引入筛选 UI

此外，在合并到代码库之前，测试必须运行并通过。Copilot 代理模式可以为你完成这些任务。现在来添加功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 依次选择 **Add Context**、**Instructions**，然后选择 **ui** 作为指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认聊天视图的代理下拉列表中仍选中 **Agent**。

   ![聊天视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选中 **Claude Sonnet 4.5**。
5. 使用以下提示词，让 Copilot 基于待办事项中的相关议题实现该功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 先探索项目，找到与所需功能相关的文件。可以看到它找到了数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会发现 Copilot 执行多项任务，如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改会统一提供 **Keep** 或 **Undo**，因此工作进行期间无需选择它们。

7. 当 Copilot 提示时，选择 **Continue** 以运行测试。

   ![Copilot Chat 窗格中对话框的屏幕截图，询问用户是否确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会出现一些停顿，甚至看到部分测试失败，这没关系。Copilot 会在代码生成与测试之间反复迭代，直到完成任务且未检测到错误。

   ![完整的 Copilot 代理模式聊天会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，审查 Copilot 或任何 AI 工具生成的代码始终很重要。

10. 返回运行网站的浏览器，探索新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！在本练习中，我们探索了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能，并了解了：

- GitHub Copilot 代理模式如何在 UI 和数据层中实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并进行协调一致的更改。
- 如何在合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其打开拉取请求。

### 附加探索练习：实现分页

随着游戏列表不断增长，需要启用分页。运用本练习中学到的技能，提示 Copilot 更新站点以实现分页。代码方面的注意事项包括：

- 遵循现有的最佳实践，包括使用现有的指令文件。
- 考虑如何实现分页，例如允许用户选择每页数量，还是将其硬编码。
- 编写提示词时，为 Copilot 提供必要的指导，使其按你的期望实现。
- 你可能需要与 GitHub Copilot 反复迭代，提出更改要求并提供上下文。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 提问、编辑和代理模式：功能与适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
