---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中体验筛选功能。现在将这些能力整合到一个自定义智能体中，并为它定义清晰的 QA 流程。

在本课程中，你将：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选功能 issue 审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的专用 Copilot。配置文件说明智能体的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义能力各有不同用途：存储库指令描述团队规范；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些能力，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，先让 Copilot 创建一个可复用的 QA 配置文件。配置文件应定义 QA 要执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，请先检查配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免配置不完整或权限过宽的智能体在你只想验证功能时更改代码。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 是可选字段，但添加后可以为智能体指定清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求开始审查、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请先让默认智能体修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话并选择 QA，以便它使用现有 issue 和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能暂时不会出现在可用智能体列表中。它仍然可用，可以通过上述命令启用。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你添加了可复用的专用角色，并审查了它的工作。在本课程中，你：

- 了解了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择了 QA 智能体，并根据筛选功能 issue 审查结果。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好等待审查。接下来，你将[整合这些更改到功能 PR，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
