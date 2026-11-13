---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们按照正确方式构建正确的功能。本课程是多个课程中的第一课，你将遵循典型的智能体开发流程：从 issue 开始实现新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能开发将继续使用同一个对话和分支。实际工作中，通常会针对不同文件类型使用不同分支或 PR；本研讨会为了聚焦核心概念，采用了简化方式。

本课程将介绍如何：

- 根据 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

在继续实现此功能的过程中，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 已描述这项功能，但在编写代码前，需要先确认组合筛选等细节。你将使用 Plan 模式确定这些细节，然后授权智能体在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变软件开发的基本原则；这些原则反而变得更加重要！大多数开发人员的工作流程大致如下：

1. 从描述需求的 issue 开始。
2. 规划需要构建的内容。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准流程，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过验证。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直至任务完成。

先进入 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能之前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

此时会话提示中应出现 `#7`（或类似编号）。`#` 可将 GitHub 上的 issue 或拉取请求 (PR) 添加到上下文。

## 规划筛选功能

在实现功能或执行任务前，规划可以帮助你先确定实现方案。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能应该如何工作。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何内容与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查完毕，现在可以开始实现。使用 Autopilot，让 Copilot 执行计划。

Autopilot 会让 Copilot 迭代解决问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 查看 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

生成代码后，和其他代码一样，必须先审查再合并。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据 issue 和规划时作出的决定审查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目验证结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但它真的能运行吗？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的筛选组合。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课程中，你：

- 根据 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[使用自定义指令][next-lesson]深入了解代码生成，确保代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
