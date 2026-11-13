---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードで絞り込み機能を作成しました。pull request を開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、絞り込み機能の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードでは、エディターの外部にあるツールを利用できるため、さらに強力になります。Copilot でそれを実現するのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部と、コンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新情報へのアクセス (リソースの利用) や、ユーザーに代わった操作 (ツールの利用) ができます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールと AI エージェントとの通信を管理します。MCP サーバーごとに、AI エージェントが利用できるツールとリソースが異なります。

よく利用される MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは新しいリポジトリを作成したり、既存のリポジトリを更新したり、Issue や pull request を管理したりできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作機能を提供します。AI エージェントは Web ページを移動したり、フォームに入力したり、ボタンを選択したりできます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくするために [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかのプロジェクト依存関係と同様に扱ってください。利用前にソースコードをよく確認し、発行元を検証し、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これは機能のテストに最適です。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` の項目を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は **ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright を画面のないヘッドレスモードで実行します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使うため、通常は Playwright 用ブラウザーもインストール済みです。後でブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを **必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときに VS Code が起動し、信頼するかを確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** 操作が表示されます。選択してサーバーを今すぐ起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するよう求められた場合は、構成を確認して、サーバーを起動するために信頼することを選択します。

手動で起動する手順は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに、VS Code がサーバーを起動して信頼するかを確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を確認して要約します。会話を中断せずに、機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントは支援技術と同じように、ラベル付きの構造化された要素 (ボタン、リンク、リスト項目など) を使って判断します。そのため、機能の簡単な確認をアクセシビリティの基本チェックとしても活用できます。

サーバーが接続され、アプリが実行中であることを確認したら、次のプロンプトを Copilot に送信して、作成した絞り込み機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue に記載された受け入れ条件に照らして報告を確認してください。問題がある場合は、pull request を開く前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> テスト時にはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使用する際に、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたら、チームでレビューする pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更されたファイルが表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## pull request を作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、pull request を作成する方法はいくつかあります。GitHub Copilot を使用しているので、PR の作成も任せましょう。Copilot は関連する Issue を見つけて、それと関連付けられた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して、新しいセッションを開始します。
2. GitHub ツールを使用できるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot に情報収集や操作を許可します。GitHub ツールを初めて使用するときに GitHub での認証を求められた場合は、画面の指示に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーで絞り込み機能を手動テストしました。
- ブランチを公開し、絞り込み機能の pull request を開きました。

MCP サーバーで機能を *テスト* しましたが、MCP は Copilot を拡張する方法のひとつです。次は、アクセシビリティのレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
