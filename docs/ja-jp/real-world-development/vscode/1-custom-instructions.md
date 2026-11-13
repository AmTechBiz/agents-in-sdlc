---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。主な方法のひとつが、コード生成に関するガイダンスを伝える[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- 既存の指示を適用した状態で、絞り込み用の最初のデータスライス (発行元ヘルパー) を作成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい規約が反映されることを確認します。
- 次の演習で Copilot が更新したガイダンスを利用できるよう、指示と絞り込み用スライスを `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わない場合があります。Copilot は非決定的に動作します。この演習では、文字単位で同じ出力になることではなく、指示を更新した後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを作成します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、提案やコードスニペットをより適切なものにできます。リポジトリ全体、または特定の種類のファイルに対するタスクレベルのコンテキストを指定できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストで Copilot に送信される単一の指示ファイルです。Copilot への多くのリクエストに関係する、プロジェクトレベルの情報を記述します。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` ファイルには、特定のタスクやファイルの種類に関するガイダンスを記述します。TypeScript や Astro などの言語、UI コンポーネントの作成、新しいユニットテストの作成といったタスクに利用できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` フロントマターが設定された `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトにある例は、一般的な方法を示しています。主なポイントは次のとおりです。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体的なコーディング規約など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指針を記述します。
- 自然な言葉を使い、明確なガイダンスとコードのよい例、悪い例を示します。

指示ファイルの作成方法も AI の使い方も、ひとつに決まっているわけではありません。プロジェクトに合った方法を、試しながら見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトの指示を確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルが用意されています。
>
> Copilot に指示ファイルを生成してもらうこともできます。方法は利用する環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な場合は、該当する環境のレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアの `copilot-instructions.md` と、タスク別の複数の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開き、内容を確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示を適用するファイルを決める glob が設定されています。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するための具体的な指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクトファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot に参考例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング規約を説明していますが、コード内のドキュメントに関する規則はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。その後に指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブがあれば閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルに変更を適用できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認して変更を適用します。ヘルパーファイルとテストなど、複数のファイルに及ぶことがあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、発行元の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードには、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが **ない** ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の動作が一貫することが重要です。

## リポジトリに新しい規約を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コーディング規約を記述して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約は記述されていますが、コード内のドキュメントについての規則はありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約の直後に、次の Markdown 行を追加します。Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に置いて全体に適用することも、特定の言語、ファイルの種類、タスク用の `*.instructions.md` ファイルとして作成することもできます。追加したドキュメントコメントの規則など、プロジェクト全体の規約にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと **同じプロンプト** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を誘導し、新しいプロジェクト規約に従わせることができました。次の演習で使うコードにも適用されます。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じように、ソース管理で管理します。指示の更新とヘルパーの変更をリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新とヘルパーの基盤に Copilot が加えたすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けて pull request を作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って、Copilot に Issue を探し、Issue を完了扱いにする PR を作成してもらいます。その後、マージを依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されています。Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。この接続の仕組み (Model Context Protocol) については、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. Copilot に指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう依頼します。PR では指示の更新に加えて、次の絞り込み機能で使う発行元ヘルパーの基盤も説明するようにします。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub 操作を **Allow** するよう求められた場合は、コマンドを確認して適切に選択します。
6. PR が作成されたら、PR をマージしてローカルのブランチを `main` に戻すよう、次のプロンプトで依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドを実行するために **Allow** を求めた場合は、内容を確認して適切に選択します。

GitHub Copilot のサポートを使って pull request を作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい規約が適用されることを確認しました。
- 次の演習で使用できるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加するときに[エージェントモード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot により適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
