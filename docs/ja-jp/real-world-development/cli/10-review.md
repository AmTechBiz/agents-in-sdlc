---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、あらかじめ用意された絞り込みの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- 絞り込みの Issue から始め、Plan モードで進め方を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証根拠を評価しました。
- 絞り込み機能の変更全体をレビューし、絞り込みの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作成します。

1. **星評価**: 既存の `starRating` と未評価の場合の表示をゲームカードに追加します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

ワークショップを簡略化するため、絞り込みの計画から PR の作成まで、同じ会話とブランチで進めました。その作業全体を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルではユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA では、それらの結果に要件とテストカバレッジの確認を加えてから PR を作成しました。

追加したテストで実際の不足を解決する必要があります。QA の実行時に新しいテストが不要でも、正しい場合があります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。アプローチを検討するときは **Plan**、範囲を絞った変更を進めながら確認するときは **Interactive**、範囲が明確なタスクを任せるときは **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。*何を*作成するのか、*なぜ*作成するのか、*どのように*作成するのかを明確に伝えると、結果は大きく変わります。

## さらに試す

基本的なワークフローは完了しました。ほかにも、次の CLI 機能を試せます。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot クラウドエージェントにタスクを送信します。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。本番コード、趣味のコード、何年も作りたいと思いながら手をつけていなかった小さなアプリなどで活用しましょう。チームと学んだことを共有し、お互いの経験から学んでください。いつものように、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の作業環境も体験する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリ ワークショップ][app]、[Copilot クラウドエージェント ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
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
