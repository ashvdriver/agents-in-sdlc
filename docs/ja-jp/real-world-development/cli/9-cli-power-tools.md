---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールの多くがそうであるように、GitHub Copilot CLI にも、操作用のスラッシュコマンドが数多く用意されています。これらのコマンドは、高度な機能、内部の情報、追加の構成オプションを提供します。`/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドは、すでに使いました。ここでは、他の便利なコマンドをいくつか確認しましょう。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストをどのように管理するかを確認する。
- `/model` を使って、利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認する。

## シナリオ

CLI の中核となるワークフローは完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、そして必要に応じて [Copilot cloud agent][about-cloud-agent] への作業の委任といった、いくつかの追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って、自分でコンテキストを確認したり圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、コンテキストがシステム指示、ツール、メッセージ、空き領域にどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話がすでに短い場合は、大きな変化はないかもしれません。

> [!NOTE]
> Copilot CLI は、ウィンドウがいっぱいになると、コンテキストを自動的に圧縮します。タイミングを自分で選びたい場合は、`/compact` を使用します。関係のないタスクに切り替えて、新しい会話にしたい場合は、`/clear` または `/new` を使用します。

## モデルを選択する

モデルごとに得意なことは異なり、開発者ごとに好みも異なります。Copilot CLI では、使用するモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、全員が AI ツールをより上手に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールされているバージョンの `/share` のオプションを確認します。
2. このセッションを共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った共有先を選択します。
3. 誰かに送信したり公開したりする前に、エクスポートされた内容をレビューします。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する意図のないリポジトリや会話の内容は、公開しないでください。

## 任意: 規模を拡大する、または委任する

ワークショップの中核部分は完了です。Copilot CLI には、より大きなタスクのためのコマンドも用意されています。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列に実行できます。
- `/worktree` は、別のタスクのために分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期で作業し、pull request を作成する場合があります。

これらのコマンドは、追加の worktree やリモートでの作業を作成する可能性があるため、任意です。試す前に、新しく、範囲が明確なタスクを開始し、結果は通常のワークフローでレビューしてください。非同期のエージェントによる作業をさらに掘り下げたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI でスラッシュコマンドを使うと、設定、セッションの共有、内部の動作の確認ができます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストをどのように管理するかを確認した。
- `/model` を使って、利用できるモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認した。

Copilot CLI には、他にも多くのスラッシュコマンドがあり、探索できる機能がまだあります。最後に、[学んだことを振り返り][next-lesson]、学習を続けるための次のステップを確認して、この旅を締めくくりましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
