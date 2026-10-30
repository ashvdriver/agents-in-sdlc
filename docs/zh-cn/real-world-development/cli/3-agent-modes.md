---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。幸运的是，GitHub Copilot CLI 可以融入组织现有的工作流，确保以正确的方式构建正确的内容。这是若干课程中的第一课，你将遵循典型的智能体驱动开发流程：从议题入手生成新功能，确保代码有效、功能行为符合预期，并最终成功合并到项目中。

> [!NOTE]
> 在功能工作流的后续课程中，你将继续使用同一个对话和分支。通常，不同类型的文件会使用不同的分支或 PR，但这里采用了简化做法，以便专注于核心概念。

首先，在本课中，你将：

- 基于 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩充，访客需要按类别和发行商缩小游戏范围。待办事项中的议题描述了该功能，但组合多个类别等细节需要先达成一致，然后才能编写代码。你将使用 Plan 模式确定这些决策，再通过 Autopilot 授权一次范围明确的实现。

## 背景

引入 AI 编码智能体不会改变开发的基本原则，反而让这些原则更加重要。大多数开发人员的工作流类似于：

1. 从描述待办工作的已提交议题开始。
2. 为需要构建的内容制定计划。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体细节因团队和组织而异，但大多是上述流程的变体。

坚持这一标准方法，可以确保 AI 生成的代码满足既定要求，并与手写代码一样经过同样的审核流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在各模式之间切换：

- **Interactive**：你与智能体协同工作。智能体提出更改建议，并在继续之前等待你的输入。
- **Plan**：智能体先制定计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并反复迭代，直至任务完成。

先在 Plan 模式中开始，审查计划，再使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，先返回 Codespace，确认存储库和终端已就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将该议题添加到提示词，并返回 **Session** 选项卡。

请注意，提示词现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求 (PR) 引入上下文。

## 规划筛选功能

规划可以让你在交给 Copilot 之前，先确定实现功能或执行任务的方法。对于任何复杂的工作，花些时间规划总是明智的。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用之后，输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 开始制定计划。它会先探索项目，再确定最佳方法。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好回答，这里没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) 展开计划。
5. 上下滚动，审查计划。
6. 让 Copilot 修改计划中与你的决策不符的部分。

## 批准 Autopilot

计划已编写并审查完毕，现在可以实现了。使用 Autopilot 让 Copilot 自行完成。

Autopilot 允许 Copilot 反复迭代，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中类似标签的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为完成。

## 审查并验证实现

代码生成后，与任何其他代码一样，需要先审查再合并。既要审查代码，也要运行站点，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与议题以及你在规划时做出的决策进行对比。
4. 代码审查完毕后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并让 Copilot 解决所有失败项。

## 探索新功能

代码看起来没问题，但能运行吗？像之前一样启动应用，并通过 Codespaces 转发的端口打开站点。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试类别筛选、发行商筛选，以及在计划中商定的组合筛选。
4. 确认重置和无结果时的行为符合议题和你的决策。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查了一项功能。在本课中，你：

- 基于 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，将[使用自定义指令][next-lesson]，深入了解代码是如何生成的，并确保其遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
