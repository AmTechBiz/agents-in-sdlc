---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你使用 GitHub Copilot 处理了多个任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在来检查代码更改；如有需要，也可以向 Copilot 提供反馈，进一步改进结果。

### 场景

正如前面多次强调的，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队启用审查之前，先查看 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## 安全性和 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此它受到一些安全限制，以确保操作安全：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟；你可以利用这段时间休息或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码，包括刚创建的 TSDoc 文档注释和其他文档。具体更改可能不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件头注释。这些内容来自练习 2 开始时审查的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查部分排队运行。如果一切正常，单个 Astro 应用程序的项目检查将通过。可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像对待团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。可以让 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的头部注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用程序的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。你可以使用上面的建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 如何执行工作。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。满意后，将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查部分排队运行。如果一切正常，单个 Astro 应用程序的项目检查将通过。可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现了相关游戏功能！和之前一样，你可以通过迭代让 Copilot 更新功能。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，在 **Conversation** 选项卡中添加评论即可提出请求，Copilot 会据此启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中通过控制面板请求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注以下内容：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查部分排队运行。如果一切正常，单个 Astro 应用程序的项目检查将通过。可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能符合预期，并且在重新加载页面后仍会保留设置。

注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理待办事项中的实际任务，让你可以先做其他工作。创建几个 issue 并分配给 Copilot，培养这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境。在这些课程中，你：

- **检查了此存储库自带的自定义指令文件**，以便稍后观察它们对云智能体输出的影响。
- **向 Copilot 云智能体分配了 issue**，并观察它如何设置环境、制定计划和异步执行任务。
- **创建并使用了自定义智能体**，为无障碍功能添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为控制面板**，监控并引导无障碍会话。
- **审查并迭代改进了云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作环境。其他工作环境可以补充本次实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，并体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
