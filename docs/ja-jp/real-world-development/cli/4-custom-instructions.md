---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を扱うときは、コンテキストが鍵になります。タスクを特定の方法で行う必要がある場合は、そのガイダンスを Copilot が利用できるようにしておきたいものです。[指示ファイル][instruction-files]には、どのようなコードが必要かだけでなく、それをどのように構造化すべきかも記述します。フィルター機能を構築したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示と、パスを限定した指示ファイルが、エージェントにどのように渡されるかを確認する。
- 指示ファイルを更新して、コーディング標準が守られるようにする。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

優れた開発チームの例に漏れず、Tailspin Toys にも開発手法に関するガイドラインと要件があります。次のような内容です。

- コメントは、コードの内容を繰り返すのではなく、意図と自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化する。注入可能な `db` 引数がある場合は、それも含める。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードを変更したときはコメントを最新の状態に保つ。
- 既存のフォーマットと lint のガイダンスを維持する。

指示ファイルを使うことで、Copilot が上記の手法に沿ってタスクを実行するために必要な情報を提供できます。

## 指示ファイル

カスタム指示を使うと、Copilot にコンテキストや好みを伝え、コーディングスタイルや要件をより深く理解させることができます。これは強力な機能で、Copilot からより適切な提案やコードスニペットを得るのに役立ちます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。指示は、リポジトリ全体向けにも、タスク単位のコンテキストを与える特定の種類のファイル向けにも作成できます。

指示ファイルには2つの種類があります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべて**のリクエストで Copilot に送信される、単一の指示ファイルです。このファイルには、ほとんどのリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイドラインを提供するファイルです。

> [!NOTE]
> その他の指示の形式とサポート状況は環境によって異なります。特定の形式に頼る前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めやすいように、スタータープロジェクトには指示ファイルが一式含まれています。変更を加える前に、すでにある内容を確認して、その影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. ファイルの内容を確認し、プロジェクトの簡単な説明とコーディングのガイダンスに注目します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに対する指示があることに注目します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドに注目します。これは、指示を適用するファイルを決定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクに注目します。これにより、大きな指示のセットを再利用可能な小さなファイルに分割し、Copilot に従うべき例を示すことができます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでに用意されているファイルは良い出発点ですが、まだ不足があります。中核となる `copilot-instructions.md` ファイルを変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにしましょう。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35行目付近にあります。
2. 次の内容を、最後の箇条書き項目として追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使用する

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルター機能の会話をいったん終了して再開します。これにより、機能のコンテキストを失わずに、Copilot が新しいガイダンスを踏まえた状態で開始できます。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを読み進めます。新しく生成された TSDoc コメントに注目し、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示と、パスを限定した指示ファイルが、エージェントにどのように渡されるかを確認した。
- 指示ファイルを更新して、コーディング標準が守られるようにした。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot - 指示ファイルなどのリソースのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
