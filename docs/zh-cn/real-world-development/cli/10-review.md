---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项以及更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在一套连续的 Tailspin Toys 工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace，安装了 Copilot CLI，探索了项目，并找到了预置的筛选功能议题。
- 添加了星级评分，在转发的浏览器中审查了结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选功能议题开始，在 Plan 模式中确定方法，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，然后自定义了现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证 (QA) 自定义智能体，用于评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并为筛选功能 PR 授权了 Agent Merge。
- 探索了用于上下文、模型、共享以及可选云委派的斜杠命令。

## 你交付了什么

本工作坊有两个 PR 里程碑，每个都基于更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选功能和质量工作流：** 实现筛选功能，更新指令并将其应用于该功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并包含相关测试。

从规划筛选功能到打开其 PR，你都使用了同一个对话和分支。我们将这些工作合并到一个 PR 中，以简化工作坊。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、自己在浏览器中检查，以及 Copilot 通过 MCP 进行的浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并以你的新格式报告结果。在 PR 之前，QA 将这些结果与需求和测试覆盖率审查结合在一起。

新增测试应弥补真正的缺口；QA 运行后无需新增测试也可能是正确的。授权合并之前，请审查代码和证据；更改之后，请刷新受影响的证据。

## 最佳实践

你提供给 Copilot 的上下文和工具决定了它的工作成果。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。请在多个对话中复用这些自定义项，并随团队需求的变化进行调整。指令设定标准，技能描述可重复的任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。请审查实际的更改和工具结果，而不只是智能体的摘要。

使**模式和模型**与任务相匹配。在构建之前使用 **Plan** 梳理方法，需要全程参与专注的更改时使用 **Interactive**，范围明确的任务使用 **Autopilot**。常规编辑选择速度更快的模型，复杂工作选择能力更强的模型。

上下文与基础设施同样重要。清晰描述*要构建什么*、*为什么*以及*如何*构建，会显著改变输出。

## 更多探索

你已涵盖核心工作流。以下几项 CLI 功能也值得一看：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：梳理问题并获得另一种视角。
- `/fleet`：并行编排相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用水平的最佳方式就是持续使用。可以把它用于生产代码、业余项目，或是你多年来一直想做却始终没动手的小应用。与团队分享你的心得，也向他们学习。当然，也要始终探索文档。

如果想在终端中扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。如需对比其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
