---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づく絞り込み機能を作成し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前のレッスンでは、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作するため、適切なものを適切な方法で作成できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスを進めます。Issue から新機能を作成し、コードの妥当性と機能の動作を確認して、最後にプロジェクトへマージします。

> [!NOTE]
> 機能のワークフローを続ける間は、同じ会話とブランチを使います。通常は、作業するファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot との新しい会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がカテゴリや発行元でゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意すべき詳細があります。Plan モードでそうした判断を行い、範囲を限定した実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの場合は上記の流れを基本にしています。

この標準的な進め方を守れば、AI が生成したコードが要件を満たし、手作業で作成したコードと同じ検証プロセスを通るようにできます。

## 会話モード

**会話モード**では、エージェントに与える自律性の度合いを設定します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次の処理に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復的に自律して作業します。

まず Plan モードで計画を確認し、その後 Autopilot モードで実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace に戻ってリポジトリとターミナルの準備をします。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てることで、機能の実装やタスクの実行を Copilot に任せる前に、進め方を決められます。複雑な作業では、事前に少し時間をかけて計画を立てることをお勧めします。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はまずプロジェクトを調べてから、最適な方法を判断し、計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に合わせて回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 判断と異なる部分があれば、Copilot に計画を修正するよう依頼します。

## Autopilot を承認する

計画を確認できたら、次は実装です。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示されている同様の項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。事前に計画を確認してください。依存関係が不足していたりポートが競合したりする場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

通常のコードと同様に、生成されたコードもマージ前にレビューする必要があります。コードをレビューしてからサイトを実行し、表示を確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力して、絞り込みの実装とテストを確認します。
3. Issue と、計画時に決めた内容に結果が沿っていることを確認します。
4. コードを確認したら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認できましたが、実際に動くでしょうか。前と同じようにアプリを起動して、Codespaces の転送ポートからサイトを開きましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリによる絞り込み、発行元による絞り込み、計画時に合意した組み合わせを試します。
4. リセット動作と結果がない場合の動作が、Issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot との新しい会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム指示を使って][next-lesson]コードの生成方法をさらに深く学び、記述されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
