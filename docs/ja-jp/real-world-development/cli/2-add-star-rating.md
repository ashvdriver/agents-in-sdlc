---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "Copilot CLI を使ってゲームカードに小さな変更を加え、転送されたブラウザーでレビューして、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したところで、いよいよプロジェクトに最初の変更を加えます。変更は小規模にします。ゲームのデータにはすでに星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示するよう依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の内容を学習します。

- フィーチャーブランチで、対象を絞った Copilot の会話を開始する。
- プロジェクトに小規模な変更を加えるようエージェントに依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、転送されたブラウザーで変更を確認する。
- 最初の pull request を作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小さくて完結した変更なので、最初のセッションに最適です。

## 会話の構造

**会話**とは、タスクのために Copilot CLI とやり取りする場です。Copilot app とは異なり、通常の CLI の会話では、専用の worktree は作成されず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチが使われます。保存した会話を使うと、後から同じやり取りに戻ることができます。ファイルとブランチは、ディスク上の通常の Git の状態のままです。

会話内には、プロンプトとエージェントの応答、ファイルを調査および編集するときのエージェントのツールアクティビティ、`/diff` で確認できる変更という3つの要素が表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot が起動していない場合は、次のコマンドで起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲームの詳細表示に使用されるファイルを見つけて、必要なコードを作成します。これで Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

AI が生成したすべての変更は、どれほど小さくてもマージ前にレビューする必要があります。Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに、評価がある場合は数値の評価が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果をレビューし、失敗があれば修正を依頼します。
5. レビューが完了したら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に決定論的ではなく確率的に動作するため、実際のコードは異なる場合があります。1つの実装と完全に一致することを期待せず、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動くと判断してはいけません。Copilot に Web サイトを起動させ、Codespaces が転送するブラウザーで更新された UI を確認しましょう。

1. アプリを起動するよう Copilot に依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. ポート `4321` が利用可能になったと Codespaces が通知したら、**Open in Browser** を選択します。
3. ゲームカードに5点満点の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。次は、新しいコードをプロジェクトにマージするための pull request (PR) を作成します。

1. デフォルトのエージェントに、変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR を作成するときに既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

お疲れさまでした。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の作業を行いました。

- フィーチャーブランチで、対象を絞った Copilot の会話を開始した。
- ゲームカードに小規模な変更を加えるようエージェントに指示した。
- `/diff` で変更をレビューした。
- アプリを実行し、転送されたブラウザーで星評価を確認した。
- 最初の pull request を作成してマージした。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使用して][next-lesson]、より大規模な機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
