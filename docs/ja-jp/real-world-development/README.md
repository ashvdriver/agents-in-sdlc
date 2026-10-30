---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成されたアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を探求します。変更の計画、コードの作成、動作のテスト、pull request のレビュー、エージェントによる作業の管理を、反復的に進めていきます。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys の新人開発者です。チームのバックログは GitHub Issue として登録済みで、すぐに取りかかれます。バックログには、フィルター機能やページネーションなどの機能開発と、アクセシビリティやコーディング標準などの品質改善が含まれています。

## 環境を選ぶ

GitHub Copilot は、普段作業している場所で利用できます。構築したいスタイルに合った環境を選んでください。各ワークショップは、それぞれのセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナル上のエージェント型アシスタントを使用します。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリで、並列エージェントセッションの実行、セッションモードの切り替え、キャンバスでの共同作業、GitHub Issue と pull request の管理を行います。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期のエージェントに割り当て、作業を導き、進行状況を監視して、エージェントが作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターから離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
