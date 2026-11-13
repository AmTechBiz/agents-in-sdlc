---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを、1 つのブランチに保存しました。すべてまとめて確認し、pull request を開きましょう。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、絞り込み機能と品質のワークフロー全体を 1 つのセッションとブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能の PR 全体と検証の根拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

絞り込みのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app で pull request をマージするために必要な残りの作業を自動化します。有効にすると、アプリのセッションは pull request を読み取り、CI チェックの失敗を修正したり、レビューコメントに対応したり、必要に応じてリベースしたりして、PR を妨げている問題を解決します。その後、GitHub で許可されるとマージします。Agent Merge はバックグラウンドで実行され、アプリを再起動しても継続し、pull request がマージされると自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることができますが、コードの編集やマージには、引き続き明示的な許可が必要です。マージを許可する前に、実行できる操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが終わりました。Agent Merge に PR のプロセスを任せましょう。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。ドロップダウンから項目を選んだだけでは、開始されません。

Agent Merge の処理が始まります。次の作業を行います。

- タイトルと説明を付けて pull request を作成します。
- Issue からセッションを開始した場合は、説明の本文で関連する Issue を参照します。
- ターゲットブランチにリベースするか、発生したマージ競合を解決します。
- CI を監視し、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot コードレビューからのフィードバックがないか PR を監視し、コメントを解決するために変更を行います。
- すべて成功したら PR を自動的にマージするよう設定できます。

すべて成功した後に Agent Merge が PR をマージするように設定しましょう。

5. マージを許可する前に、PR の差分と検証の根拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のようにエージェントに明示的に依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進捗を確認し、報告された問題を解決します。続行前に、PR の状態が **Merged** になっていることを確認します。マージを許可したことやエージェントが実行中であることは、マージ完了の確認にはなりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリ保護や権限の不足を回避することはできません。続行する前に、そうした問題を解決してください。

## まとめと次のステップ

コード生成、テスト、検証に加えて、pull request のプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能の PR 全体と検証の根拠を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は、[既存のキャンバスを使い、トリアージ用キャンバスを作成][next-lesson]して、エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue と pull request を管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
