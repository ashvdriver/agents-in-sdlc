---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能的实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一个分支上。现在可以一并审查它们并打开拉取请求。星级评分拉取请求 (PR) 由你亲自合并；这一次，将由 **Agent Merge** 管理该流程。

> [!NOTE]
> 通常，我们会将功能、指令更新、技能更新和 QA 智能体拆分为几个独立的 PR。为简化工作坊，你将完整的筛选和质量工作流保留在同一个对话和分支中，所有这些工作都将纳入此 PR。

在本课中，你将：

- 了解 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动完成剩余的 PR 工作，同时由开发人员掌握合并授权。

## Agent Merge 简介

**Agent Merge** 可自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会逐一处理阻碍 PR 合并的问题，包括修复失败的持续集成 (CI) 检查、回应审查意见，以及在需要时变基，然后在存储库允许时启用 GitHub 自动合并。

到目前为止，你都是自己选择 **Merge pull request**。Agent Merge 可以接管这项职责。启用 Agent Merge 之前，请先审查工作成果，并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部创建完成后，一并审查，创建 PR，并让 Agent Merge 管理其余流程。

1. 返回 Codespace。
2. 输入 `/agent`，打开智能体对话框。
3. 在选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监视 PR 上的持续集成流程。一切成功后，它会执行合并。
7. 确认看到 Copilot 发出类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或缺失的权限。如果被阻止，请阅读报告的原因；在存储库允许时，经审查后手动完成合并。

## 总结与后续步骤

你已自动化开发流程的多个环节，包括生成代码、测试和验证代码，以及现在的拉取请求流程。你：

- 了解了 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，涵盖上下文、模型、共享以及可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
