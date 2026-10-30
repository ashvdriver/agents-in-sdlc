---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与所有优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多用于交互的斜杠命令。这些命令可提供高级功能、后台信息和更多配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来了解其他几个实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用的模型。
- 了解如何使用 `/share` 导出或共享会话。
- 了解用于并行工作、工作树以及委派给云智能体的可选命令。

## 场景

你已完成 CLI 的核心工作流。现在来看看其他几项功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会触及模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令自行检查或压缩。

1. 返回 Codespace，如果 Copilot CLI 尚未打开，请在存储库根目录中启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前令牌用量，以及上下文在系统指令、工具、消息和可用空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能并不明显。

> [!NOTE]
> 窗口填满时，Copilot CLI 会自动压缩上下文。想自行选择压缩时机时，请使用 `/compact`。切换到不相关的任务并希望开启全新对话时，请使用 `/clear` 或 `/new`。

## 选择模型

不同的模型各有所长，不同的开发人员也有不同的偏好。Copilot CLI 可让你列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用的模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享心得，有助于每个人改进使用 AI 工具的方式。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享的链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择符合需求的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给他人或发布之前，请先审查导出的内容。会话导出内容可能包含提示词、响应和项目详情。

发布链接或 gist 是可选的。请勿发布团队不打算共享的存储库或对话内容。

## 可选：扩展或委派

核心工作坊到此结束。Copilot CLI 还提供了用于处理较大任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行运行。
- `/worktree` 可为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体，它会异步工作，并可能打开拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。尝试之前，请先启动一个全新且范围明确的任务，并通过常规工作流审查结果。如果想深入了解异步智能体工作，请继续学习[云智能体工作坊][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 中的斜杠命令，可以配置它、共享会话，并了解后台发生的情况。在本课中，你：

- 使用 `/context` 和 `/compact` 了解了 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用的模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 了解了用于并行工作、工作树以及委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令等待探索。让我们通过[回顾所学内容][next-lesson]并了解继续学习的后续步骤，结束这段旅程。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
