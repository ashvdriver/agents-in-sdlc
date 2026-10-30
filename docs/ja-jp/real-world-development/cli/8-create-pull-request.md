---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1つのブランチに保存されています。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを、いくつかの別々の PR に分割します。ワークショップを効率的に進めるため、フィルター機能と品質に関するワークフロー全体を1つの会話とブランチにまとめており、その作業のすべてがこの PR に含まれます。

このレッスンでは、次の内容を学習します。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学ぶ。
- 機能の変更全体と検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビューの後でのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフロー全体を通して、Copilot を使って機能の計画、実装、検証を行いました。Tailspin Toys では、マージの承認を開発者の管理下に置いたまま、残りの PR の作業を自動化したいと考えています。

## Agent Merge の紹介

**Agent Merge** は、pull request を取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。失敗している継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

ここまでは、**Merge pull request** を自分で選択していました。Agent Merge は、その役割を引き受けることができます。Agent Merge を有効にする前に、作業をレビューし、準備ができていると判断してください。

## Agent Merge で PR を管理する

すべてのコードが作成できたので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に管理させましょう。

1. Codespace に戻ります。
2. `/agent` を入力して、エージェントのダイアログを開きます。
3. オプションの一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は、PR の継続的インテグレーションのプロセスを監視します。すべてが成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、不足している権限を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されていれば、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、コードのテストと検証、そして pull request のプロセスなど、開発プロセスのいくつかの部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学んだ。
- 機能の変更全体と検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビューの後でのみ Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ、[Copilot CLI のその他の便利なスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
