---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，整合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选功能的体验。现在，你将把这些能力整合到一个自定义智能体中，并为其设定明确的 QA 流程。

本课将介绍如何：

- 了解自定义智能体如何与指令、技能和 MCP 工具协同工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并对照筛选功能议题审查它的发现。

## 场景

Tailspin Toys 希望在打开拉取请求 (PR) 之前，对需求、代码质量、自动化检查、测试覆盖率和浏览器行为进行一致的审查。自定义智能体可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述了智能体的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你已使用过的各项自定义功能各有分工。存储库指令描述团队标准，`quality-checks` 技能封装可重复的检查，Playwright MCP 提供浏览器工具，而 QA 配置文件则告诉 Copilot 如何运用这些能力评估需求并报告发现。它不会取代这些功能，也无需另开对话。

## 创建 QA 配置文件

打开功能 PR 之前，先让 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体之前，先审查其配置文件，确认 Copilot 已写明预期的 QA 工作流和边界。这样可以避免不完整或权限过宽的智能体在你只想验证功能时修改功能。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但填写后智能体会有清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 从需求出发、遵循存储库指令、运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会报告支持性证据，更改实现代码前会先询问，并且不会提交更改或打开拉取请求。
5. 如果生成的配置文件缺少上述任何职责或边界，请先让默认智能体修改，再继续。

## 对照议题运行 QA

Copilot CLI 在对话开始时加载项目智能体。创建配置文件后，请恢复同一个筛选功能对话，然后选择 QA，使其能够利用上下文中已有的议题和规划决策。

1. 使用以下提示词启用该智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了该智能体，它可能不会显示在可用智能体列表中。它确实存在，上述命令可以将其激活。

2. 使用以下提示词，让 QA 智能体审查该功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体开始工作。
4. 它完成工作后，通读其提供的报告。

## 总结与后续步骤

你已在工作流中添加了一个可复用的专家角色，并审查了它的工作成果。在本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协同工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并对照筛选功能议题审查了它的发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来，你将[把它们整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
