---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能の追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザインチームが改善を検討できる関連ゲーム機能を作成し、ハイコントラストモードとライトモードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

生成 AI を加えても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進める必要があります。チーム全体にレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは、監視されていない状態で非同期に作業するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード作成用のブランチに限られます。
- クラウドエージェントは GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、Copilot クラウドエージェントが最初に作成した pull request (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは **想定された動作** です。必要に応じて [Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザータブで GitHub.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることもあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. pull request の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントが追加されたコードを確認します。実際の変更内容は異なる場合があります。コードを読むときは、演習 2 の最初に確認したカスタム指示ファイルで指定した TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。
6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request の作業は一方通行ではありません。チームメンバーと同じように、pull request のコメントやコードのインラインコメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、具体的な依頼文をあらかじめ指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を説明する短いコメントヘッダーを追加する。
- TypeScript ファイルや Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにタグ付けし、生成されたドキュメントに変更を依頼するコメントを追加します。上記の例や、コードベースに追加したいドキュメントについて、別の変更を依頼してもかまいません。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して、pull request に戻ることもできます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更を確認します。

納得できるまで反復して改善してください。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージできます。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲームの詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request のチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (Copilot が作成したブランチ名、たとえば **copilot/fix-8** に `<branch-name>` を置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot クラウドエージェントに変更を依頼して反復できます。たとえば、表示する関連ゲームの数や、コメントヘッダーと TSDoc のドキュメントコメントの追加を依頼できます。後者を依頼する場合は、カスタム指示を更新する前に Issue を割り当てたことを思い出してください。これまでと同様に **Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - ハイコントラストモードとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えを試します。ページを再読み込みしても設定が保存されることを確認してください。

アクセシビリティのカスタムエージェントによって、Copilot がアクセシビリティのベストプラクティスに従って機能を実装できました。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** をタグ付けして変更を依頼できます。

## 任意の演習 — 委任を続ける

クラウドエージェントには、実際のバックログ項目を任せて、その間に別の作業を進める方法が適しています。この方法を習慣にするため、リポジトリに Issue を追加して Copilot に割り当ててみましょう。たとえば、次のような項目があります。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントのハーネスを完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、クラウドエージェントの出力への影響を確認しました。
- **Issue を Copilot クラウドエージェントに割り当て**、非同期に環境をセットアップして計画し、実行する様子を確認しました。
- **カスタムエージェントを作成して使用**し、ハイコントラストモードとライトモードを追加しました。
- **Copilot エージェント ページをミッションコントロールとして使用**し、アクセシビリティ セッションを監視して作業中に指示を出しました。
- **クラウドエージェントの pull request をレビューして反復改善**し、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

クラウドエージェントのハーネスを完了しました。さらに学びたい場合は、次のハーネスも体験してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` によるクラウドエージェントとの連携を使い、レビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次の方法も試してみましょう。

- リファクタリングやテストカバレッジの Issue をクラウドエージェントに割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
