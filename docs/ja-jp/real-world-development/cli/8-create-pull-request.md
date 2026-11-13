---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを、1 つのブランチに保存しました。すべてまとめて確認し、pull request を開きましょう。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、絞り込みと品質のワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能の変更全体と検証の根拠を確認します。
- 絞り込みの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込みのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正したり、レビューコメントに対応したり、必要に応じてリベースしたりして、PR を妨げている問題を解決します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。今後は Agent Merge にその役割を任せることができます。有効にする前に、作業をレビューして問題がないと判断してください。

## Agent Merge で PR を管理する

コードの作成が終わりました。まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功したら、マージを実行します。
7. 「PR #14 was squash-merged successfully」のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避することはできません。処理がブロックされた場合は、報告された理由を確認し、リポジトリの設定で許可される場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コード生成、テスト、検証に加えて、pull request のプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能の変更全体と検証の根拠を確認しました。
- 絞り込みの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
