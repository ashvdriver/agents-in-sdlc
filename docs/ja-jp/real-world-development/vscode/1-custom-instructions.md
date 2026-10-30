---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を扱うときは、コンテキストが鍵になります。タスクを特定の方法で行う必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストに確実に到達できるようにしておく必要があります。特定のコンテキストを Copilot と共有する方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]で、コード生成に関するガイダンスを提供する手段です。

この演習では、次の内容を行います。

- プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が、リポジトリのカスタム指示とパスを限定した指示ファイルを通じて、Copilot にどのように届くかを確認する。
- *現在の*指示を適用した状態で、フィルター機能の最初のデータスライス (パブリッシャーのヘルパー) を生成する。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加する。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従う様子を確認する。
- 次の演習で Copilot が更新後のガイダンスを使用できるよう、指示の更新とフィルター機能のスライスを `main` にコミットする。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部から外れる場合があります。Copilot は非決定論的です。この演習の目的は、指示を更新した後の動作の*傾向*の変化を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発チームの例に漏れず、Tailspin Toys にも開発手法に関するガイドラインと要件があります。次のような内容です。

- データレイヤーには、常に単体テストが必要である。
- UI はダークモードで、モダンな雰囲気にする。
- ドキュメントは、TSDoc ドキュメントコメントの形式でコードに追加する。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加する。

指示ファイルを使うことで、Copilot が上記の手法に沿ってタスクを実行するために必要な情報を提供できます。

### カスタム指示

カスタム指示を使うと、Copilot にコンテキストや好みを伝え、コーディングスタイルや要件をより深く理解させることができます。これは強力な機能で、Copilot からより適切な提案やコードスニペットを得るのに役立ちます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。指示は、リポジトリ全体向けにも、タスク単位のコンテキストを与える特定の種類のファイル向けにも作成できます。

指示ファイルには2つの種類があります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべて**のリクエストで Copilot に送信される、単一の指示ファイルです。このファイルには、プロジェクトレベルの情報、つまり Copilot に送信されるほとんどのチャットや CLI のリクエストに関係するコンテキストを記述します。使用している技術スタック、構築中のものの概要、ベストプラクティス、その他の全体的なガイダンスなどを含められます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類向けに作成できるファイルです。特定の言語 (TypeScript や Astro など) のガイドラインや、UI コンポーネントや新しい単体テストの作成といったタスクのガイドラインを提供できます。

> [!NOTE]
> IDE で作業している場合、指示ファイルは Copilot Chat でのコード生成にのみ使用されます。コード補完や次の編集の提案には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コードを生成するときに、リポジトリレベルのファイルと、(`applyTo` フロントマターを持つ) `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は、AGENTS.md ファイルや CLAUDE.md ファイルなど、[他の標準を使用した指示ファイルにも対応しています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成についての詳しい説明は、このワークショップの範囲を超えます。ただし、サンプルプロジェクトで提供されている例は、代表的なアプローチを示しています。大まかには、次のとおりです。

- `copilot-instructions.md` の指示は、構築中のものの説明、プロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` ファイルは、ファイルの種類 (単体テスト、Astro コンポーネント、データレイヤー) や特定のタスクに対する、個別の指示を提供するために使用します。
- 自然言語を使用します。ガイダンスは明確にします。コードがどうあるべきか (そして、どうあるべきでないか) の例を示します。

AI の使い方が1つに決まっていないのと同様に、指示ファイルの作り方も1つに決まっているわけではありません。実験を通じて、プロジェクトに最適な方法が見つかります。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトには、充実した指示ファイルのコレクションが必要です。このプロジェクトのファイルを調べると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまな種類のタスク向けのファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルの生成を任せることもできます。この方法は、サーフェスごとに異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`)。使用しているサーフェスのレッスンで、関連する箇所で説明します。
>
> テンプレートや出発点をお探しですか。指示ファイル、カスタムエージェント、その他のリソースが豊富にそろったリポジトリ、[awesome-copilot][awesome-copilot] を探索してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれている指示ファイルを読んでみましょう。中核となる `copilot-instructions.md` が1つと、さまざまなタスク向けの `*.instructions.md` ファイルのコレクションがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. ファイルの内容を確認し、プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションに注目します。**Code standards** の下にある、入れ子の **GitHub Actions Workflows** のガイダンスにも注目します。これらは、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、中を見て回ります。Astro ファイル、Drizzle データレイヤー、テストなどに対する指示があることに注目します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドに注目します。これは、指示を適用するファイルを決定する glob (リポジトリルートからの相対パス) を設定します。ここでは、あらゆる TypeScript のテストファイル (たとえば `**/*.test.ts` に一致するもの) が一致します。
5. このプロジェクトの単体テストの作成に固有の指示に注目します。
6. 最後に、`.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル (`unit-tests.instructions.md` など) や、プロジェクト内の既存のファイルへのリンクに注目します。これにより、大きな指示のセットを再利用可能な小さなファイルに分割し、コードを生成するときに従うべき例を Copilot に示すことができます。(そこでのパスは、リポジトリルートではなく、指示ファイルからの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須になっていません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従って、変更用のブランチを作成しましょう。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使用する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して、新しい会話を開始してください。モードとモデルの選択は演習ごとに異なります。必要な箇所で、各手順で説明します。

カスタム指示の影響を確認するために、まず、現在の指示ファイルを適用した状態でプロンプトを送信します。後で指示ファイルを更新し、同じプロンプトを再送信して違いを確認します。

1. 必要なコンテキストだけが Copilot に取り込まれるよう、前の演習から開いているエディターのタブをすべて閉じます。
2. ヘルパーを配置する場所を Copilot が把握できるよう、`src/lib/publishers.ts` を開きます。
3. Copilot がファイルの変更を適用できるよう、Chat ビューのエージェントのドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードの更新を適用します。多くの場合、ヘルパーファイルとそのテストにまたがります。
6. 提案されたヘルパーが、第1引数に `db` クライアントを受け取り、型付きのパブリッシャーの配列を返す、型付きの関数であることに注目します。これは、`.github/instructions/drizzle.instructions.md` (`src/lib/*.ts` に適用されます) にあるデータレイヤーの規約に由来します。
7. 提案されたコードに、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**含まれていない**ことに注目します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合でも問題ありません。指示を更新した後の*一貫性*の向上を確認することが目的です。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するように設計されています。リポジトリのコーディング標準を文書化して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を再び開きます。
2. **Code formatting requirements** セクションを探します。27行目付近にあります。プロジェクトのコーディング標準が文書化されていますが、コード内のドキュメントに関するルールがまだないことに注目してください。生成されたヘルパーにドキュメントコメントがなかったのは、このためです。
3. 既存の標準のすぐ下に次の Markdown の行を追加し、ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは、全体的なガイダンス向けにはリポジトリレベル (`.github/copilot-instructions.md`) で、特定の言語、ファイルの種類、タスク向けには `*.instructions.md` ファイルとして作成できます。たった今追加したドキュメントコメントのルールのような、プロジェクト全体の標準は、リポジトリレベルのファイルに記述するのが適切です。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` をもう一度クリックして、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが含まれるようになったことに注目します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれるようになったことに注目します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を誘導して新しいプロジェクト標準に従わせ、次の演習で土台となる実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルは、リポジトリ内の他のアセットと同じように、他のものと同じソース管理の方法で管理します。そこで、ブランチをコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパーの土台のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたので、pull request を作成し、バックログにあるドキュメント標準の Issue と関連付けます。手動でも行えますが、プロジェクトにすでに接続されている GitHub ツールを使って、Copilot に代行させることもできます。Issue の検索、その Issue をクローズする PR の作成、マージを Copilot に依頼しましょう。

> [!NOTE]
> これは、Copilot がユーザーに代わって GitHub 上で操作を行う最初の演習です。プロジェクトテンプレートは、GitHub のツールをワークスペースにすでに接続しているため、セットアップは不要です。Copilot が初めてツールを使用するときに、VS Code が GitHub へのサインインを求める場合があります。プロンプトに従って許可してください。この接続の仕組み (Model Context Protocol) は、後の演習で学習します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を見つけ、指示の更新と新しいパブリッシャーのヘルパーの土台の両方を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を開始します。
5. Copilot がユーザーに代わって GitHub の操作を実行することを **Allow** するよう求められたら、コマンドを確認し、適切であれば **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って、PR のマージとブランチを main に戻すことを Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot がユーザーに代わって GitHub の操作とシェルコマンドを実行することを **Allow** するよう求められたら、コマンドを確認し、適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りて、pull request を作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、その後 VS Code の Copilot Chat を使って次の作業を行いました。

- コード生成のプロンプトを送信し、*既存の*指示を適用した状態で Copilot が生成する内容を確認した。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従う様子を確認した。
- 次の演習で活用できるよう、指示の更新とヘルパーの土台を `main` にコミットした。

次は、[エージェントモード][next-lesson]で、Copilot がコードベース全体に新機能を追加する際に、これらの指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズ用の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot - 指示ファイルなどのリソースのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
