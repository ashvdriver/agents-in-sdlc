---
title: "第 12 课 - 使用 GitHub Pages 部署站点"
description: "使用 GitHub Copilot app 配置 Pages 工作流，审查其拉取请求，并验证已上线的 Space Quiz。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

通过 GitHub Pages 部署 Space Quiz，为它提供一个公开的访问地址。Copilot 可以准备工作流并引导部署，但你仍需审查更改，并确认站点已经上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署过程，并验证已发布的站点。

## 场景

Space Quiz 已经在 GitHub 存储库中，但人们还需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件站点，无需添加服务器或运行时依赖项，即可通过公开 URL 访问。

## 让 Copilot 部署站点

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功的 GitHub Actions Deploy website 运行，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的 Space Quiz 线上站点。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示会要求智能体创建并合并拉取请求，还会更改存储库的 Pages 设置。继续之前，请先审查差异、部署权限和所有 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认仅在存储库中尚无 README 时才添加 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 必须在所需的拉取请求检查全部通过后才能合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开智能体报告的 URL，确认 Space Quiz 能够正常加载。

如果智能体无法启用 Pages 或合并拉取请求，请按它给出的具体说明操作。只有部署成功且线上站点能够加载，任务才算完成。

## 总结和后续步骤

Space Quiz 现已作为线上站点提供访问，并且你已亲自验证部署，而不是仅依赖智能体的报告。继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
