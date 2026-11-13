---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，应确保 Copilot 能获取相关指导。你可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

在本练习中，你将：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在现有指令下生成第一个筛选数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级规范。
- 再次运行相同提示，观察生成的代码如何应用新规范。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的部分规范。Copilot 的输出具有概率性。本练习旨在让你观察更新指令前后 Copilot 行为的*变化趋势*，而不是要求输出逐字相同。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，你可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库，以及希望在代码中使用的注释类型，从而引导 Copilot 提供更相关的建议和代码片段。你可以为整个存储库创建指令，也可以为特定任务或文件类型创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。该文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

入门项目已包含一组指令文件。它们展示了典型的做法。总体而言：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目内容、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，清晰地编写指导，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过实践，你会逐步了解适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应包含完善的指令文件。浏览本项目中的文件时，你会发现其中有适用于多种任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同工作环境的操作方式有所不同，例如 VS Code 中的 **配置聊天 → 生成智能体指令**，或 Copilot CLI 中的 `/init`。相应课程会在介绍相关功能时说明操作方式。
>
> 需要模板或起点？浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间查看此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及用于多种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些指导适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置了相对于存储库根目录的 glob，用于确定指令适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看此项目中有关创建单元测试的专门指导。
6. 最后，打开 `.github/instructions/drizzle.instructions.md`。注意其中链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目文件。这有助于将较大的指令集拆分成更小、可复用的文件，并提供项目示例供 Copilot 参考。路径相对于指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目的编码规范，但目前还没有要求代码中包含文档。接下来，你将添加 TSDoc 文档注释和文件头注释的规则。

## 为更改创建分支

现在遵循最佳实践，为更改创建分支。

1. 从上一练习返回 codespace。
2. 打开新的终端，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同；每个步骤都会在需要时说明。

要观察自定义指令的影响，先在当前指令文件下发送提示，稍后更新指令后，再次发送相同提示并比较结果。

1. 关闭之前练习打开的所有编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件头注释。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有指示，它也可能添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新指令后行为的一致性改进。

## 添加存储库级规范

前面提到过，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码规范，提高代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。注意其中记录了项目编码规范，但没有代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如前一课所介绍的，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级规范，例如刚刚添加的文档注释规则。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件开头有类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目规范，并将其应用于真实代码；下一练习会继续使用这项基础工作。

## 提交指令并推送分支

指令文件与存储库中的其他文件一样，使用相同的源代码管理流程。现在将指令更新和辅助函数基础代码提交并推送到存储库。

1. 打开新的终端窗口，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已保存：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础代码所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建拉取请求，并关联待办事项中的文档规范 issue。你可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。向 Copilot 发送提示，让它查找 issue、创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经在工作区配置了 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接机制 Model Context Protocol (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选择了 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础代码：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果系统提示是否允许 Copilot 代表你执行 GitHub 操作，请审查操作并在适当时选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR 并将当前分支切回 main，发送以下提示：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示是否允许 Copilot 代表你执行 GitHub 操作或运行 shell 命令，请审查操作并在适当时选择 **Allow**。

你已经借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，然后在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察现有指令下的生成结果。
- 向 `.github/copilot-instructions.md` 添加新的存储库级规范。
- 再次运行相同提示，观察生成的代码如何应用新规范。
- 将指令更新和辅助函数基础代码提交到 `main`，供下一练习继续使用。

接下来，让这些指令发挥作用，学习如何使用[智能体模式][next-lesson]跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
