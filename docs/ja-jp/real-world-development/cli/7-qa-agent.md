---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上の絞り込み機能を確認しました。次は、明確な QA プロセスを定義したカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成し、内容を確認します。
- QA エージェントを選択し、絞り込みの Issue に照らして報告を確認します。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化バージョンです。プロファイルにはエージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示ではチームの規約を説明します。`quality-checks` スキルでは繰り返し実行できるチェックをまとめます。Playwright MCP ではブラウザーツールを利用できます。QA プロファイルでは、これらを使って要件を評価し、結果を報告するよう Copilot に指示します。QA プロファイルは各機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを Copilot に作成してもらいます。プロファイルでは QA の実行内容と守るべき範囲の両方を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を記述していることを確認します。これにより、確認だけが目的なのに不完全なエージェントや範囲の広すぎるエージェントが機能を変更することを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名を分かりやすくできます。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたり pull request を開いたりしないことを確認します。
5. これらの責任や制約が不足していたら、続ける前に既定のエージェントへ修正を依頼します。

## Issue を使って QA を実行する

Copilot CLI は会話の開始時にプロジェクトエージェントを読み込みます。プロファイルを作成した後、絞り込みの会話を再開して QA を選択すると、すでに共有されている Issue と計画の判断を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトを使って、機能のレビューを QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、報告を確認します。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成し、内容を確認しました。
- QA エージェントを選択し、絞り込みの Issue に照らして報告を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、それらをまとめて機能の PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
