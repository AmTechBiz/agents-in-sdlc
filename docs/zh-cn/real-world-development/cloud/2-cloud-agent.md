---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务。这些问题可能是尚未解决的安全问题、需要更新的旧代码，或因为时间不足而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成工作后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

你将使用 Copilot 云智能体了解以下内容：

- 如何自定义代码生成环境。
- 如何确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 如何向 Copilot 分配 issue。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，你会发现文档完全缺失。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已经准备好改善游戏发现体验。他们希望每款游戏的详情页显示相关游戏，也就是属于相同类别的其他游戏。暂时不需要完善的实现，只需要可用于 UX 验收测试的功能。虽然这项工作目前受阻，但还有优先级更高的问题。

这两项任务都可能被搁置，因此很适合分配给 Copilot 云智能体。Copilot 云智能体可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式与它协作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看 Copilot 会话。提出的解决方案准备好等待审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的指令

GitHub Copilot 并非魔法工具，你仍需思考任务要求。即使看似简单的操作，也可能包含复杂细节，因此[应仔细界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没完成，现在可以快速查看。分配任务前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一部分将 **Code lacks documentation** issue 分配给云智能体时，留意它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件头注释。这些要求来自你查看过的指令文件。稍后的练习还会再次介绍这一点。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态良好。分配任务给 Copilot 时也一样，因为它会像软件工程师一样处理任务。

云智能体在 GitHub Actions 中运行。你可以通过创建专用设置工作流来定制环境，该工作流位于 `.github/workflows/copilot-setup-steps.yml`，可在 Copilot 开始工作前运行，以安装所需工具和依赖项。实验室已预先配置此工作流，以确保流程顺利并提供学习机会。它会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器，并为单个 Astro 应用程序迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此工作流看起来与其他 GitHub 工作流相同，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它可以让你从存储库的 **Actions** 选项卡手动运行工作流，方便先验证工作流，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的文档不是过时，就是完全缺失。这类技术债务经常无人处理，降低工作效率，也增加维护代码库或引入新团队成员的难度。Copilot 擅长创建文档，因此很适合将这项任务分配给 Copilot 云智能体。它会在后台生成所需文档；你将在之后的练习中审查这些工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办事项列表和 Copilot 即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的方案，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们有时间处理其他工作。现在就利用这一点：再分配一项任务给 Copilot 云智能体，然后自己编写代码，为应用程序添加功能。

## 为游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一项任务，它负责另一项。为设计团队添加相关游戏部分可能不会花太长时间，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二项请求！Copilot 云智能体的工作方式类似于软件工程师，因此你不需要持续监控，而是在完成后审查即可。接下来，我们将了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课程介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将 issue 分配给 Copilot，让它异步处理任务，例如解决技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 如何自定义代码生成环境。
- 如何确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 如何向 Copilot 分配 issue。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
