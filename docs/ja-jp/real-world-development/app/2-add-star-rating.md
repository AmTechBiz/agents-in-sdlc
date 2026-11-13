---
title: "レッスン 2 - 星評価を追加する: 小さな改善から始める"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

前のレッスンでは、ワークスペースを確認し、クイックチャットを使いました。次は **エージェントセッション** を開始し、プロジェクトに最初の変更を加えます。変更は小さくしましょう。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、セッションの構成を確認します。
- エージェントにプロジェクトへの小さな変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリ、発行元、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。初めてのセッションにぴったりの、小さく独立した変更です。

## セッションの構成

**セッション**とは、それぞれ独立したワークスペースで動作するエージェントとの会話です。各セッションには**専用の Git worktree とブランチ**が割り当てられます。そのため、一方のセッションで機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを実行できます。セッションはリポジトリごとにサイドバーにまとめて表示されます。セッションを選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調べて編集する際のエージェントの**ツール操作**、差分が表示された**変更ファイル一覧**があります。

## セッションを開始して変更を依頼する

新しいセッションを始めて、プロジェクトの調査と機能の実装に取りかかります。[アプリの設定][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリの新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能の追加に必要なファイルを見つけて、コードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューしてください。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択すると、変更されたファイルとその差分が表示されます。

    ![Create PR ボタンの右にある Toggle review panel ボタンを矢印で示した、GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示をテストしていることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合は成功とは見なせません。インストールの要求は内容を確認してから承認してください。

コードを読んだだけで動くと判断せず、Copilot に Web サイトを開いてもらい、更新された UI を確認しましょう。Web サイトを起動し、ブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べ、自分でも作成しますが、ここでは組み込みのブラウザーキャンバスを使用します。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに 5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初の pull request を作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするための pull request (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. pull request を作成する前に該当する Issue が必要であると Copilot から通知された場合は、そのプロセスに従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで pull request を開きます。ここで PR をレビューできます。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、pull request をマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更をリリースできました。このレッスンでは、次のことを行いました。

- エージェントセッションを開始し、セッションの構成を確認しました。
- エージェントにゲームカードへの小さな変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[絞り込みの Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェントセッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue と pull request を管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして設定する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
