---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 获得相应的指导。[指令文件][instruction-files]不仅描述你想要*什么*代码，还描述代码应*如何*组织。现在筛选功能已经构建完成，你将了解 Copilot 使用的指令，添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和路径范围的指令文件如何传达给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

与任何优秀的开发团队一样，Tailspin Toys 制定了开发实践方面的准则和要求，包括：

- 注释应解释意图和不易看出的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录其用途、参数和返回值，其中如有可注入的 `db` 参数也应一并说明。
- 可复用的 Astro 组件应记录其 `Props` 约定，且相关代码更改时应同步更新注释。
- 应保留现有的格式和 lint 指导。

通过指令文件，你可以确保 Copilot 掌握正确的信息，按上述实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，让它更好地理解你的编码风格和要求。这项功能可以帮助你引导 Copilot，获得更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望在代码中包含的注释类型。可以为整个存储库创建指令，也可以针对特定类型的文件创建指令，提供任务级上下文。

指令文件分为两种：

- `.github/copilot-instructions.md`：单个指令文件，会随存储库的**每次**请求发送给 Copilot。该文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索此项目中的自定义指令文件

为便于入门，入门项目已包含一组指令文件。更改之前，先了解现有内容，以便观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览该文件，留意其中对项目的简要说明和编码指导。这些指令适用于此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。可以看到针对 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他指令文件和现有项目文件的链接。这样可以将较大的指令集拆分为更小的可复用文件，并为 Copilot 指明可参考的示例。

## 更新指令文件以匹配团队指导

现有文件是不错的起点，但仍有缺口。修改核心文件 `copilot-instructions.md`，确保为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到标题为 **Code formatting guidance** 的部分，大约在第 35 行。
2. 在其末尾添加以下列表项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 在对话开始时加载存储库指令。保存更新后的指令后，请退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时，从新指导开始工作。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认已检出 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，通读已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确解释了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能。具体而言，你：

- 了解了存储库指令和路径范围的指令文件如何传达给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
