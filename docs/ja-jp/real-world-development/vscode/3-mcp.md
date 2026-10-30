---
title: "演習 3 - Playwright MCP サーバーによる機能のテスト"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。pull request を作成する前に、ブラウザーで実際に動作することを確認する必要があります。アプリを自分で操作して確認する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot をどのように拡張するかを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright MCP サーバーを使わせて、ブラウザーでフィルター機能を手動でテストする。
- ブランチを公開し、フィルター機能の作業に対する pull request を作成する。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外に手を伸ばせるようになると、はるかに強力になります。Model Context Protocol (MCP) は、Copilot がそれを行うための仕組みで、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部の仕組みと、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との関わりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、(リソースを使用して) 最新の情報にアクセスし、(ツールを使用して) ユーザーに代わって操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスの橋渡し役となる MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や、NPM パッケージなどのローカルツール) の間の通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる、それぞれ異なるツールとリソースのセットを表します。

広く使われている既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: このサーバーは、GitHub リポジトリを管理するための一連の API へのアクセスを提供します。AI エージェントは、新しいリポジトリの作成、既存のリポジトリの更新、Issue や pull request の管理などの操作を実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: このサーバーは、Playwright を使用したブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンのクリックなどの操作を実行できます。

他にも、さまざまなツールやリソースへのアクセスを提供する MCP サーバーが数多くあります。GitHub は、エコシステムの発見しやすさと貢献を高めるために、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをプロジェクトの他の依存関係と同様に扱ってください。MCP サーバーを使用する前に、ソースコードを慎重にレビューし、発行元を確認して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密性の高いリソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されているはずです。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれており、前の演習で Copilot が Issue のバックログを読み取れたのはこのためです。これは[リモートの GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールするものはありません。VS Code は HTTP でこれに接続し、Copilot がそのツールを初めて使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が制御できるブラウザーを提供します。これは、機能をテストするために必要なものです。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**のサーバーです。VS Code は、`npx @playwright/mcp@latest` を実行して、ご自身のマシンでサーバーを起動します。`--headless` フラグは、ブラウザーを表示ウィンドウなしで実行するよう Playwright に指示します。これは、表示するデスクトップのない codespace 内で必要です。

> [!NOTE]
> Tailspin Toys プロジェクトは、エンドツーエンドテストにすでに Playwright を使用しているため、Playwright が必要とするブラウザーは、通常すでにインストールされています。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**オンデマンド**で起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかの確認を求めます。

1. `.vscode/mcp.json` の `playwright` エントリの上に、**Start** アクションが表示されます。これを選択すると、サーバーをすぐに起動して、構成が正しいことを確認できます。
2. サーバーを信頼するかどうかの確認が VS Code から求められたら、構成を確認し、サーバーを起動できるよう信頼することを選択します。

手動での起動は任意です。省略した場合は、次の手順で Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。作業内容を確認するために自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を報告できます。その後、確認した内容を要約します。会話から離れずに、機能が期待どおりに動作することを確認する、最も素早い方法です。

内部的には、Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]に基づいて動作します。つまり、エージェントは、支援技術と同じように、構造化されたラベル付きの要素 (ボタン、リンク、リスト項目) に基づいて判断します。そのため、簡単な機能チェックが、簡易的なアクセシビリティの健全性チェックも兼ねます。

サーバーが接続され、アプリが実行されている状態で、構築したフィルター機能を実行するよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各ステップを順に実行して、確認した内容を報告します。その要約を、Issue の受け入れ基準と照らして読んでください。おかしな点があれば、追加で質問するか、pull request を作成する前にコードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再び起動してください。Copilot が Playwright MCP サーバーを初めて使用するときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたので、チームがレビューできるように pull request (PR) を作成します。最初の手順は、`filtering-vscode` ブランチの公開です。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを使って、コミットメッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

pull request を作成する方法は、github.com や GitHub コマンドラインインターフェイス (CLI) など、いくつかあります。ただし、すでに GitHub Copilot を使って作業しているので、PR の作成も Copilot に任せましょう。Copilot は、関連する Issue を見つけて、それに関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の収集や操作の実行に必要なタスクを Copilot が実行できるよう、必要に応じて **Continue** を選択します。GitHub ツールを初めて使用するときに、GitHub での認証を求められる場合があります。プロンプトに従って許可してください。
5. Copilot が Issue を検索し、適切なものを見つけて、PR を作成する様子に注目します。
6. Copilot が生成したリンクを選択して pull request をレビューしますが、**まだマージしないでください**。

## まとめと次のステップ

お疲れさまでした。この演習では、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot をどのように拡張するかを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- それを使って、リリースする前にブラウザーでフィルター機能を手動でテストした。
- ブランチを公開し、フィルター機能の作業に対する pull request を作成した。

MCP サーバーを使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の1つにすぎません。次は、[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなどの焦点を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ誰もが話題にしているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
