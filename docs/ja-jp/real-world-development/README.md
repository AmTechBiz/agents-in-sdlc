---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更を計画し、コードを作成して、テストし、pull request をレビューし、エージェントと作業を管理する反復的なプロセスを体験します。

## シナリオ

あなたは、ボードゲームのクラウドファンディングを開発者向けテーマで提供する架空の会社、Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。カテゴリやページネーションの絞り込みなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップはそれぞれセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを活用します。

### [GitHub Copilot アプリ][app]

複数のエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、デスクトップアプリで GitHub Issue と pull request を管理します。

### [GitHub Copilot クラウドエージェント][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、作成された pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
