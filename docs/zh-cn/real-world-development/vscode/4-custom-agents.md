---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

借助 GitHub Copilot 中的[自定义代理][custom-agents-concept]，可以创建针对开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，引导它更有效地执行特定类型的工作。团队可以将自己的专业知识固化为可复用的代理，例如强制遵守 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可在 `~/.copilot/agents` 中全局定义。每个文件都带有 YAML frontmatter，其中至少包含 `name` 和 `description`，后跟定义代理行为、专业知识和指令的 Markdown 提示词。

### 自定义代理与代理技能的比较

自定义代理与[代理技能][agent-skills-concept]在逻辑上有一些重叠。两者主要都由 Markdown 文件定义，并告诉 AI 如何执行操作。最清晰的区分方式是：**自定义代理**是执行者，**技能**是工具。

自定义代理拥有自己的上下文窗口，旨在工作过程中编排技能（甚至其他代理）。在本实验中，无障碍自定义代理会依据无障碍准则审查并更新站点；在此过程中，它可以调用诸如拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一“正确”的方法。与 AI 领域的其他事物一样，请通过测试和迭代，找到适合你的环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户无障碍，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景色之间的对比度不足，部分用户觉得当前的深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现一个用户可以开关的高对比度模式。

由于无障碍至关重要，你希望尽快完成这一功能。你将使用自定义代理来生成该功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已为你创建了无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并基于 `main` 切换到一个新分支，用于无障碍工作（筛选功能 PR 来自练习 3，请与之保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义代理必须包含带有 `name` 和 `description` 的 frontmatter。

3. 然后浏览并查看后面的各个部分，其中重点介绍：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 的代码示例。
   - 常见陷阱和错误清单。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中，列出 `.github/agents` 中定义的每个自定义代理。选择某个自定义代理，即可将聊天会话限定在该代理的指令和工具范围内。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习之前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。模式和模型的选择因练习而异，各步骤会在需要时说明。

1. 如果尚未选中，请在聊天视图的代理下拉列表中选择 **Agent**。

   ![聊天视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉列表（其中显示当前代理，默认为 **default**）。
3. 在可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 开始工作：它会探索代码库、提出更改建议，并将编辑应用到项目文件。每次编辑都会内联显示在聊天中，附带文件路径和可供查看的差异。
6. 随着编辑不断落地，**Files changed** 指示器会更新，让你看到代理修改过的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在对你的存储库进行真实的更改，工作时会编辑 Astro 组件、CSS 和所有相关测试等现有文件。

你将在下一个练习中审查并引导这项进行中的工作。

## 总结与后续步骤

本课探索了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域的专用 AI 助手。借助自定义代理，可以将团队的专业知识和标准固化为可复用的代理，引导 Copilot 更有效地执行特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理的工作][next-lesson]：在更改发生时审查它们，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
