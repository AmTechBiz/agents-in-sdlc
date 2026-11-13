---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能の要望などがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、エージェントは人間の開発者がレビューできるドラフト PR を作成します。こうした方法で面倒なタスクを任せると、開発を加速し、より重要な作業に集中できます。

このレッスンでは、Copilot クラウドエージェントを使って次のことを行います。

- コードを生成する環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確にした Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトの作成を担当した開発者が、ドキュメントを適切に整備しませんでした。コードにドキュメントがほとんどありません。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルなど、関連ゲームを表示して、訪問者がゲームを見て回れるようにします。現時点では完成された実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、さらに優先度の高い Issue があります。

いずれも優先順位が低くなりやすいタスクの例です。Copilot クラウドエージェントに割り当てると、非同期に作業を任せて、開発者は別のタスクに集中できます。その後、Copilot の作業に戻ってレビューし、期待どおりか確認できます。

## GitHub Copilot クラウドエージェントとは

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト pull request を作成し、作業環境をセットアップして、タスクに着手します。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、Copilot が pull request で通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく調べると複雑な部分が見つかります。[Copilot クラウドエージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot に対する役割を学びました。すでにカスタム指示の演習を行った場合は、`.github/copilot-instructions.md` が機能していることを確認したはずです。まだの場合は、この機会に簡単に確認しましょう。Copilot クラウドエージェントに作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認して、後から影響を確認できるようにします。

GitHub の Web UI で次のファイルを開きます。Codespace が起動している場合は、Codespace で開いてもかまいません。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関するガイダンスを確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、指示が `**/*.test.ts` に一致するファイルに限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue をクラウドエージェントに割り当てるとき、作成される pull request に TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後の演習で PR をレビューするときに、もう一度説明します。

## Copilot クラウドエージェントの開発環境を設定する

誰が作業する場合でも、コードを作成する際は通常、開発環境の準備やスクリプトの実行が必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

クラウドエージェントは、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルに[特別なセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行される環境をカスタマイズし、必要な開発ツールや依存関係を使用できるようにします。このラボでは、学習をスムーズに進められるよう、事前に設定されています。単一の Astro アプリで、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとデータ投入済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot には操作用の独自のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローに似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot が pull request の作業を始める前に、このジョブが GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の作業を待たずに、正常に実行できるかをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債を放置すると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成を得意としているため、クラウドエージェントに依頼するのに適したタスクです。Copilot はバックグラウンドで必要なドキュメントを生成します。後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. リストと、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているかを確認します。コードを調べて状態を理解し、適切な計画を考え、コードを作成します。

これには数分かかる場合があります。Copilot クラウドエージェントの主な目的は、作業を非同期に実行し、ユーザーが別のタスクに集中できるようにすることです。別のタスクも Copilot クラウドエージェントに割り当て、その間にアプリケーションに機能を追加する作業を進めましょう。

## ゲーム詳細ページに関連ゲームを追加する

説明したように、GitHub Copilot クラウドエージェントの利点のひとつは、作業を分けられることです。デザインチームのために関連ゲームを追加するのに時間がかからない場合でも、その時間を別の作業に使えます。Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューすればよいのです。次は、カスタムエージェントを作成して使う方法を確認しましょう。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]について学びました。クラウドエージェントに Issue を割り当て、非同期に作業させることができます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに活用できます。

このレッスンでは、次のことを行いました。

- コード生成の環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

クラウドエージェントがバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot クラウドエージェントは MCP サーバー][cloud-agent-mcp]も使用できます。また、前のモジュールで説明したカスタム指示も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
