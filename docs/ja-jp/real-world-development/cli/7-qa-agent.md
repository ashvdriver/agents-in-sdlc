---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに、`quality-checks` スキルで自動チェックを実行し、Playwright MCP でフィルター機能の操作感をブラウザーで確認しました。今度は、これらの機能を、明確に定義された QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが、指示、スキル、MCP ツールとどのように連携するかを確認する。
- 再利用可能な品質保証 (QA) プロファイルを作成して確認する。
- QA エージェントを選択し、フィルター機能の Issue に照らして指摘事項をレビューする。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストのカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントは、この QA プロセスを取りまとめ、再利用可能なレポートを提供できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義される、Copilot の特化版です。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ってきたカスタマイズには、それぞれ異なる役割があります。リポジトリの指示は、チームの標準を記述します。`quality-checks` スキルは、繰り返し実行できるチェックをパッケージ化します。Playwright MCP は、ブラウザーツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、指摘事項を報告する方法を Copilot に伝えます。それらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。プロファイルには、QA が実施するチェックと、守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. デフォルトのエージェントに、新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、プロファイルをレビューして、意図した QA ワークフローと境界を Copilot が反映していることを確認します。これにより、機能を検証したいだけのときに、不完全だったり範囲が広すぎたりするエージェントが機能を変更してしまうのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略できますが、含めるとエージェントに分かりやすい表示名を付けられます。
3. プロファイルの指示を読み、QA が要件から開始し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が裏付けとなる証拠を報告し、実装コードを変更する前に確認を求め、変更のコミットや pull request の作成を行わないことを確認します。
5. 生成されたプロファイルにこれらの責務や境界が不足している場合は、続行する前に、デフォルトのエージェントに修正を依頼します。

## Issue に照らして QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後で同じフィルター機能の会話を再開し、QA を選択します。これにより、QA はすでにコンテキストにある Issue と計画時の決定事項を利用できます。

1. 次のプロンプトを使って、エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりなので、利用可能なエージェントの一覧に表示されない場合があります。エージェントは存在しており、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が終わったら、エージェントが提供するレポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが、指示、スキル、MCP ツールとどのように連携するかを確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA エージェントを選択し、フィルター機能の Issue に照らして指摘事項をレビューした。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートがレビューできる状態になりました。次は、[それらを機能の PR にまとめ、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
