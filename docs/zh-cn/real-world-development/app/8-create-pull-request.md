---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支上。现在一并审查这些内容并创建拉取请求。你已手动合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理该流程。

> [!NOTE]
> 通常，我们会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化研讨会流程，筛选和质量工作流都保留在同一个会话和分支中，所有更改都会放入这个 PR。

在本课程中，你将：

- 了解 Agent Merge 及其如何自动化 PR 合并生命周期。
- 检查完整的功能更改和验证证据。
- 完成审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的开发过程中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动化 PR 的后续工作，同时让开发人员掌握合并授权权。

## 了解 Agent Merge

**Agent Merge** 会自动处理 GitHub Copilot app 中拉取请求合并前的剩余工作。启用后，app 会话会读取 PR 并解决阻塞事项，包括修复持续集成 (CI) 检查失败、回应审查意见，以及在需要时变基；GitHub 允许后，它会执行合并。该流程在后台运行，app 重启后仍会继续，并会在 PR 合并后自动关闭。

此前由你手动选择 **Merge pull request**。Agent Merge 可以接手这项工作，但编辑代码和合并仍需要明确授权。授予合并权限前，请审查它获准执行的操作和当前工作。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完毕，现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮开始流程。仅选择下拉菜单中的选项并不会启动该流程。

Agent Merge 流程启动后会：

- 创建包含标题和说明的拉取请求。
- 如果会话从 issue 启动，则在说明正文中引用相关 issue。
- 对目标分支执行变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控其他开发人员或 Copilot 代码审查的反馈，并进行更新以解决意见。
- 可以选择在所有操作成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进进度并解决报告的阻塞事项。继续之前，确认 PR 状态为 **Merged**；仅启用合并权限或智能体仍在运行，并不代表已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前，请先解决这些阻塞事项。

## 总结和后续步骤

你自动化了开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，你将[使用现有画布并创建待办事项分类画布][next-lesson]，探索使用智能体检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
