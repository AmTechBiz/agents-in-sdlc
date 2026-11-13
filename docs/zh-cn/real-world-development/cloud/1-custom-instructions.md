---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 应了解某些背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你想做什么，也了解应该如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会比较提示的前后效果。Copilot 云智能体会异步处理 GitHub issue，因此很难实时并排展示差异。你将在后续练习中审查云智能体创建的拉取请求，并看到指令文件的影响。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。该文件应包含项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如为 TypeScript、Astro、UI 组件或单元测试提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何编写指令文件的完整讨论超出了本研讨会的范围。不过，示例项目展示了典型的做法。总体而言：

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
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部。注意它链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目文件。这有助于将较大的指令集拆分成更小、可复用的文件，并提供项目示例供 Copilot 参考。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目的编码规范，但目前还没有要求代码中包含文档。接下来，你将添加 TSDoc 文档注释和文件头注释的规则。

## 添加存储库级规范

接下来，你将进行本练习唯一的编辑：添加一项项目级规则，要求代码使用 TSDoc 文档注释和文件头注释。云智能体稍后处理你分配的 issue 时会应用这项规则。

编辑前，先创建并切换到新分支（云智能体会读取 issue 目标分支中的指令；你仍应规范地提交自己的更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前面提到过，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，提高代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。注意其中记录了项目编码规范，但尚未包含代码文档要求，因此之前生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如前一课所介绍的，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级规范，例如刚刚添加的文档注释规则。

## 提交、推送并合并指令更新

云智能体会读取 issue 目标分支中的指令。下一练习中，将 issue 分配给 Copilot 时，它会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，云智能体在下一练习中处理分配给它的 issue 时就能读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处采用分支方式，是为了模拟真实项目中的工作流程。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了一项存储库级规范。在后续练习中，你会看到云智能体生成的拉取请求如何应用该规范。

接下来，[将第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
