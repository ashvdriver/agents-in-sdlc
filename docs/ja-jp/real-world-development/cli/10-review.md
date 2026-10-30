---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローを通して、GitHub Copilot CLI を使用しました。具体的には、次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調べ、用意されたフィルター機能の Issue を見つけた。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初の pull request (PR) を手動でマージした。
- フィルター機能の Issue から開始し、Plan モードでアプローチを定義して、Autopilot モードで構築し、Interactive モードでレビューした。
- カスタム指示でエージェントをガイドした後、既存の `quality-checks` スキルをカスタマイズし、それを使って単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を探索した。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価した。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を承認した。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認した。

## リリースしたもの

このワークショップには2つの PR のマイルストーンがあり、それぞれ更新した `main` から作成した専用のブランチにあります。

1. **星評価:** 既存の `starRating` と、評価がないことを示す明示的な状態を、ゲームカードに表示します。
2. **フィルター機能と品質のワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを効率的に進めるため、その作業を1つの PR にまとめました。

## 検証方法の違い

コードは、自動テスト、ご自身によるブラウザーでの確認、MCP を通じた Copilot によるブラウザーでの探索など、複数の方法で確認しました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、PR を作成する前に、それらの結果を、要件とテストのカバレッジのレビューとまとめました。

追加するテストは、実際の不足を埋めるものであるべきです。新しいテストが不要な QA の実行が正しい場合もあります。マージを承認する前に、コードと証拠をレビューし、変更後は、影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、その作業を形作ります。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらのカスタマイズは、会話をまたいで再利用し、チームのニーズの変化に合わせて調整してください。指示は標準を定め、スキルは繰り返し行うタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールを接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合わせて**モードとモデル**を選択します。構築前にアプローチを検討するには **Plan**、焦点を絞った変更で作業に関わり続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使用します。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

コンテキストは、インフラと同じくらい重要であることに変わりはありません。何を、なぜ、どのように構築したいのかを明確に説明すると、出力が大きく変わります。

## さらに探索する

中核となるワークフローは一通り学びました。他にも、確認する価値のある CLI の機能がいくつかあります。

- `/review`: コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck`: 問題を話しながら整理し、別の視点を得ます。
- `/fleet`: 独立したサブタスクを並列に調整します。
- `/worktree`: 別のタスクを分離します。
- `/delegate`: タスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールも、上達する最善の方法は使い続けることです。本番のコードにも、趣味のコードにも、何年も前から頭にあったのに作らずにいた小さなアプリにも使ってみてください。学んだことをチームと共有し、チームの学びからも学びましょう。そして、いつものように、ドキュメントを探索してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
