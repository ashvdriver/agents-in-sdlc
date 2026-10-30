---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、その pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でも使える場所に置きます。Copilot はワークフローの準備とデプロイの案内を手伝えますが、変更内容のレビューとサイトが公開されたことの確認はご自身の責任で行います。

このレッスンでは、次の内容を学習します。

- GitHub Pages のデプロイを設定するよう Copilot に依頼する。
- マージする前に、ワークフローと pull request をレビューする。
- デプロイの進行状況を追い、公開されたサイトを確認する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、誰かが気軽に試せる手段はまだありません。1ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイムの依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3段階の図: マージされた space-quiz の pull request、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、そして yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントに pull request の作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。先に進む前に、差分、デプロイの権限、GitHub の承認プロンプトを確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がまだない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルドとデプロイのステップを含め、pull request の変更内容をレビューします。
3. 必須の pull request チェックに合格してからマージします。
4. GitHub Actions のデプロイ実行を、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効にできない場合や pull request をマージできない場合は、エージェントが示す具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまでは、タスクが完了したとは見なさないでください。

## まとめと次のステップ

Space Quiz が公開サイトとして利用できるようになりました。しかも、エージェントの報告に頼らず、デプロイをご自身で確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
