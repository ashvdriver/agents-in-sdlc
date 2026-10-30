---
title: "レッスン 6 - Playwright MCP による機能の検証"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を探索するために使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに触れたように、ソフトウェア開発はコードを書くことだけではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにする必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がそれをどのように使うかを理解する。
- Playwright MCP サーバーがまだ利用できない場合は追加する。
- ブラウザーを操作してフィルター機能を探索するよう、エージェントに依頼する。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。作業中の Web サイトをユーザーと同じように Copilot に使わせることで、変更の進め方をさらに自動化し、更新が期待どおりに動作するという確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、最新の情報にアクセスし、ユーザーに代わって操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスの橋渡し役となる MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントがアクセスできる、それぞれ異なるツールとリソースのセットを表します。

広く使われている既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub のリポジトリ、Issue、pull request を管理するための API へのアクセスを提供します。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使用したブラウザー自動化機能を提供します。

他にも多くの MCP サーバーが利用できます。GitHub は、エコシステムの発見しやすさと貢献を高めるために、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同様に扱ってください。使用する前に、ソースコードをレビューし、発行元を確認して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう、追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、これらのセットアップ手順をスキップして、次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類として **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を探索するよう Copilot に依頼する

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って、同じことを行いましょう。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> MCP サーバーを使うよう Copilot に指示する必要は、実際にはありません。通常は Copilot が自分で判断します。ただし、使うべきものがわかっているなら、方向性を示しておくのも悪くありません。結果がより安定し、トークンも少し節約できます。

2. Copilot が、機能が動作することを確認するためにブラウザーで実行しているさまざまなステップを一覧表示する様子を見守ります。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止し、レポートを提供します。

## まとめと次のステップ

お疲れさまでした。Playwright MCP サーバーを使って、Copilot CLI から実際のブラウザーで機能を探索しました。ここまでの内容を振り返ります。

- Model Context Protocol (MCP) とは何か、Copilot CLI がそれをどのように使うかを学習した。
- Playwright MCP サーバーがまだ利用できなかったため、追加した。
- ブラウザーを操作してフィルター機能を探索するよう、エージェントに依頼した。

次は、[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ誰もが話題にしているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
