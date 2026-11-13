---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションです。エージェント主導の開発に必要な機能を、ひとつの作業環境にまとめています。複数のエージェントセッションの並列実行、セッションモードの切り替え、共有キャンバス、GitHub Issue と pull request のネイティブな管理機能を備えています。また、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまで pull request を導く **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールして、リポジトリを接続します。ワークスペースと、あらかじめ用意されたバックログも確認します。
2. 星評価の小さな変更を行い、ブラウザーで結果を確認して、最初の pull request (PR) を手動でマージします。
3. 絞り込みの Issue から始め、**Plan** モードで進め方を決め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、絞り込み機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成して、要件、カバレッジ、検証の根拠をレビューします。
8. 絞り込み機能の変更全体をレビューし、2 つ目の PR に Agent Merge を使用します。
9. キャンバスが利用可能か確認し、Database Explorer を使用した後、リポジトリと連携するトリアージキャンバスを作成してテストします。必要な機能が利用できない場合は、その制約を記録して読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップを簡略化するため、作成する PR は 2 つです。1 つ目は星評価、2 つ目は指示、スキル、QA プロファイル、テストの更新を含む絞り込み機能です。どちらも更新済みの `main` から始めます。絞り込みと品質のワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学びながら、それまでの作業を活用できます。最後のキャンバス演習は同じセッションに残し、PR ワークフローを繰り返さずに、共有画面を作成してテストすることに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善から始める][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、PR 1 をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、絞り込み機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | Customize から MCP を設定し、絞り込みの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家のプロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 絞り込み、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使用してから、リポジトリと連携するトリアージキャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、今後のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認定学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスでも最後まで完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。このポリシーは Copilot CLI ポリシーとは別です。

## はじめに

**[前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
