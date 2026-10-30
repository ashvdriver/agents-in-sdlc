---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

将 GitHub Copilot 置于代理模式时，它会自主工作：探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作实时发生在你的机器上，VS Code 中的 Copilot Chat 会实时显示每次工具调用和每次文件编辑。你可以内联查看每个差异、接受或拒绝单个更改，并通过后续提示词引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话中途引导代理，在高对比度之外添加浅色模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了额外的需求：他们还希望为觉得默认主题过暗的用户提供浅色模式开关。无需从头开始新对话，你可以引导现有会话来扩展这项工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每次文件编辑都会内联显示在聊天中，附带差异以及 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 代理工作时，滚动聊天记录，观察每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的所有终端命令。
3. 代理提出文件编辑时，差异会内联显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 还原更改，让代理重新处理。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在此会话期间修改过的完整工作集。

> [!NOTE]
> 如果某次工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式的实现。你应会看到一条汇总其所做更改的最终消息。

## 引导代理添加浅色模式

高对比度模式就绪后，你将扩展同一对话，添加浅色模式开关。由于继续使用同一聊天会话，代理会保留先前工作的全部上下文，包括它修改过的组件、使用的样式方法，以及建立的本地存储模式。

1. 在 Copilot Chat 输入框中，发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会从上次中断的地方继续，查看它已做的更改，并提出浅色模式的新增内容。
3. 与高对比度模式一样，在每个新编辑落地时审查它们，并对每个差异使用 **Keep** 或 **Undo**。
4. 如果代理的方向不符合你的预期，例如以你不喜欢的方式重构开关 UI，可以使用聊天输入框旁边的 **Stop** 按钮停止它，然后发送说明性的后续消息。

> [!TIP]
> 引导进行中的会话比重新开始更快。可以使用后续提示词请求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式集成到现有设置面板中”），或扩大范围（“同时为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作成果之前，快速检查代理触及的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。应会看到对 Astro 组件、样式和所有相关测试的更新。
3. 打开几个已更改的文件，逐个查看差异。确认自定义代理中的无障碍模式已得到体现，包括 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储实现的持久化。
4. 满意后，在 Source Control 面板中暂存并提交更改。你将在[下一课][next-lesson]中发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 代理模式监控并引导了进行中的会话：在每次文件编辑落地时进行审查，接受或拒绝更改，并发送后续提示词，在不丢失上下文的情况下扩展工作。

在本课中，你了解了：

- 实时监控代理的工作。
- 接受和拒绝单个文件编辑。
- 使用后续提示词在会话中途进行引导。

在[下一课][next-lesson]中，你将把无障碍工作作为拉取请求发布，并审查本地自定义代理在整个实验中产生的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
