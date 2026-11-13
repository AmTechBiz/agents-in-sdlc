---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体可以异步自主工作，但仍然需要监控这些任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制面板中，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，还可以引导正在运行的会话。

在本课程中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

在分配高对比度模式任务后，团队意识到也可以同时添加浅色模式。由于已经在更新网站样式并添加切换功能，将其一并实现很合理。现在，你将引导智能体的工作，让它在实现高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建运行环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务的各种操作。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时查看右侧显示的 PR 窗格。在此处可以查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

了解当前运行的任务后，让 Copilot 在实现高对比度模式的同时也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![控制面板中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始实际工作。看到类似下方的消息时，就说明已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了在 Copilot 工作时引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示，并将其纳入后续工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！发送消息时，Copilot 正在执行的步骤完成后，会把新请求纳入工作流程。

与之前一样，这需要几分钟。等待时，可以暂停片刻，回顾目前学到和体验的内容。

## 总结和后续步骤

本课程介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过该控制面板跟踪所有活动和已完成的任务、审查正在执行的工作，还可以调整正在进行的任务范围或提供其他指导。

你了解了以下内容：

- 使用控制面板和智能体页面监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来你将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
