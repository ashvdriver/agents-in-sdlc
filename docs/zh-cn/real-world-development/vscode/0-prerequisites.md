---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习之前，需要先做好各项准备。你将创建自己的 Tailspin Toys 存储库副本，启动一个 [codespace][codespaces] 作为工作环境，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验存储库

为存放你将创建的代码，需要基于[模板][template-repository]创建一个实例。新实例包含实验所需的全部文件，你将在完成各项练习时使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template** 按钮，创建自己的存储库副本。然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 主办的活动中完成本工作坊，请按导师提供的说明操作。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下你创建的存储库路径（**organization-or-user-name/repository-name**），稍后在实验中会用到。

> [!NOTE]
> **待办事项已就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题作为待办事项。在整个工作坊中，你将基于这些议题开展工作，无需自行提交。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 导航到刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟时间，但这仍比手动安装所有服务快得多。你可以利用这段时间探索 GitHub Copilot 的其他功能，我们接下来就会介绍。

> [!CAUTION]
> 后续练习中还会用到该 codespace。目前请在浏览器的一个标签页中保持其打开。

> [!NOTE]
> 本工作坊设计为在 codespace 或本地[开发容器][dev-containers]中运行。两者都能确保环境已安装所有必需的先决条件，使体验更加顺畅。如果希望在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

要使用 GitHub Copilot Chat 代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用的是 GitHub Codespace，则应已安装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以[从 Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息并按 Enter。这应会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 账户。请按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，应会出现 Copilot Chat 窗口。

## 总结

恭喜，你已创建实验存储库的副本，并已开始创建 codespace。开始编写代码时，你将使用该 codespace。

## 后续步骤

现在开始让 Copilot 发挥作用。继续学习[练习 1 - 自定义指令][next-lesson]，向 Copilot 传授项目的约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 快速入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
