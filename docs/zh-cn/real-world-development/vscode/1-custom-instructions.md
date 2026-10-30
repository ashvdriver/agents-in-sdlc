---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解某些背景信息，就应确保这些上下文能被获取。与 Copilot 共享特定上下文的方式有多种，其中最主要的是[指令文件][instruction-files]，它们用于提供有关代码生成的指导。

在本练习中，你将：

- 了解项目特定的上下文、编码准则和文档标准如何通过存储库自定义指令和路径范围的指令文件传达给 Copilot，
- 在*当前*指令下生成筛选功能的第一个数据切片（发行商辅助函数），
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准，
- 重新运行相同的提示词，观察生成的代码采用新标准，
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一个练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与你设定的部分标准不一致，因为 Copilot 具有不确定性。本练习的重点是观察更新指令后行为的*变化趋势*，而不是逐字符匹配输出。

## 指令文件

### 场景

与任何优秀的开发团队一样，Tailspin Toys 制定了开发实践方面的准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 应在每个文件的开头添加一段注释，说明该文件的作用。

通过指令文件，你可以确保 Copilot 掌握正确的信息，按上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，让它更好地理解你的编码风格和要求。这项强大的功能可以帮助你引导 Copilot，获得更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望在代码中包含的注释类型。可以为整个存储库创建指令，也可以针对特定类型的文件创建指令，提供任务级上下文。

指令文件分为两种：

- `.github/copilot-instructions.md`：单个指令文件，会随存储库的**每次**请求发送给 Copilot。该文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或 CLI 请求相关的上下文，例如所用的技术栈、所构建内容的概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md` 文件：可针对特定任务或文件类型创建。可用它们为特定语言（如 TypeScript 或 Astro）提供准则，或为创建 UI 组件、新的单元测试集等任务提供准则。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 中的代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，既会使用存储库级文件，也会使用 `*.instructions.md` 文件（带有 `applyTo` frontmatter）。
>
> 此外，Copilot [支持使用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

关于创建指令文件的完整讨论超出了本工作坊的范围，但示例项目中提供的示例展示了具有代表性的方法。概括而言：

- 让 `copilot-instructions.md` 中的指令专注于项目级指导，例如所构建内容的说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体指令。
- 使用自然语言，保持指导清晰，并提供代码应该（以及不应该）是什么样子的示例。

创建指令文件没有唯一的方法，就像使用 AI 没有唯一的方法一样。你会通过实验发现最适合自己项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览此项目中的指令文件时，你可能会注意到其中包含针对多种任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。各个界面的实现方式不同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`），你所用界面对应的课程会在相关位置说明。
>
> 想找模板或起点？请探索 [awesome-copilot][awesome-copilot]，这个存储库中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

花点时间阅读此存储库附带的指令文件：一个核心的 `copilot-instructions.md`，以及一组针对各类任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。可以看到针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个（相对于存储库根目录的）glob，用于确定指令适用于哪些文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意专门针对此项目创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分为更小的可复用文件，并在生成代码时为 Copilot 指明可参考的示例。（其中的路径是相对于指令文件而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加文档。在接下来的步骤中，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循一些最佳实践，为更改创建一个分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) 打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 在更新指令*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习之前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。模式和模型的选择因练习而异，各步骤会在需要时说明。

要观察自定义指令的影响，请先在当前指令文件生效的情况下发送提示词。稍后更新指令并重新发送相同的提示词，对比差异。

1. 关闭之前练习中打开的所有编辑器标签页，让 Copilot 只获取你想要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在聊天视图的代理下拉列表中选择 **Agent**，让 Copilot 能够应用文件更改。

   ![聊天视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常涉及辅助函数文件及其测试。
6. 留意所建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 留意所建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> 由于 Copilot 具有概率性，即使没有被要求，它也有可能添加文档注释。如果出现这种情况也没关系，关键仍是更新指令后*一致性*的改善。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 旨在向 Copilot 提供项目级信息。接下来确保存储库的编码标准已记录在案，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约在第 27 行附近。留意它如何记录项目的编码标准，但尚无代码内文档的规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准的正下方添加以下 Markdown 行，指示 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，可以在存储库级别创建指令文件（`.github/copilot-instructions.md`）来提供全局指导，也可以创建 `*.instructions.md` 文件，针对特定语言、文件类型或任务提供指导。存储库级文件最适合存放项目级标准，例如你刚添加的文档注释规则。

## 重新运行提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除缓冲区。
2. 点击回到 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意所建议的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意所建议的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码，下一个练习将在此基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资产一样，使用与其他项目相同的源代码管理方式进行管理。现在提交并将分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) 打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已就位：

   ```bash
   git status
   ```

3. 在终端中，暂存并提交指令更新，以及 Copilot 为辅助函数基础而更改的每个文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中，将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并将其与待办事项中的文档标准议题关联。可以手动完成，但 Copilot 可以使用已连接到项目的 GitHub 工具代劳。让 Copilot 查找该议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一个让 Copilot 代你在 GitHub 上执行操作的练习。项目模板已将 GitHub 的工具接入你的工作区，因此无需任何设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub，请按提示允许。你将在后续练习中了解这一连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) 打开 Copilot Chat 视图，并确保在代理选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并基于当前分支创建 PR，说明指令更新以及新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找议题并创建 PR。
5. 当系统提示是否**允许** Copilot 代你执行 GitHub 操作时，请查看命令，并酌情选择 **Allow**。
6. PR 创建后，使用以下提示词，让 Copilot 合并 PR 并将你的分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当系统提示是否**允许** Copilot 代你执行 GitHub 操作和运行 shell 命令时，请查看命令，并酌情选择 **Allow**。

至此，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何从此项目的指令文件获取上下文，然后使用 VS Code 中的 Copilot Chat 完成了以下操作：

- 发送代码生成提示词，观察 Copilot 在*现有*指令下的输出，
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准，
- 重新运行相同的提示词，观察所建议的代码采用新标准，
- 将指令更新和辅助函数基础提交到 `main`，以便下一个练习在此基础上继续。

接下来，当 Copilot 在整个代码库中添加新功能时，你将在[代理模式][next-lesson]下运用这些指令。

## 资源

- [用于 GitHub Copilot 自定义的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好的自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
