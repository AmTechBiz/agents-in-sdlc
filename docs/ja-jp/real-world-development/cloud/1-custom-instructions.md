---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に求めることだけでなく、その実行方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更の前後に同じプロンプトを実行しません。Copilot クラウドエージェントは GitHub Issue に対して非同期で作業するため、その影響をリアルタイムで並べて示すのが難しいからです。ハーネスの後半でクラウドエージェントが作成した pull request をレビューするときに、指示ファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストで Copilot に送信される単一の指示ファイルです。Copilot への多くのチャットや CLI リクエストに関係する、プロジェクトレベルの情報を記述します。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` ファイルには、特定のタスクやファイルの種類に関する指針を記述します。TypeScript や Astro などの言語、UI コンポーネントの作成、新しいユニットテストの作成といったタスクに利用できます。

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
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクトファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、コード生成時に参考例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング規約を説明していますが、コード内のドキュメントに関する規則はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## リポジトリに新しい規約を追加する

次に、この演習で行う唯一の編集として、ドキュメントを TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーとしてコードに含める、プロジェクト全体の規則を追加します。後でこのハーネスで Issue を割り当てるとき、クラウドエージェントはこの規則を読み取ります。

編集の前に、ブランチを作成します。クラウドエージェントは Issue の対象ブランチにある指示を読み取りますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コーディング規約を記述して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング規約は記述されていますが、コード内のドキュメントについての規則はありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約の直後に、次の Markdown 行を追加します。Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に置いて全体に適用することも、特定の言語、ファイルの種類、タスク用の `*.instructions.md` ファイルとして作成することもできます。追加したドキュメントコメントの規則など、プロジェクト全体の規約にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

クラウドエージェントは、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、クラウドエージェントが指示を読み取れるよう、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けて pull request を開き、マージします。次の演習で Issue に割り当てたとき、クラウドエージェントはこれらの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットできます。ここでは実際のプロジェクトの進め方に近づけるため、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。残りのハーネスでクラウドエージェントが作成する pull request で、この規約を確認できます。

次は、[最初の Issue を Copilot クラウドエージェントに割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot により適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
