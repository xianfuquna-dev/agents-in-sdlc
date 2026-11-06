---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到一个自定义智能体中，定义清晰的 QA 流程。

本课将：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，然后在 Copilot CLI 中选择它。

你使用过的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 使用这些能力来评估需求并报告发现。它不会取代其他配置，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，先要求 Copilot 创建可复用的 QA 配置文件。配置文件将定义 QA 要执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查其配置文件，确认 Copilot 理解了预期的 QA 流程和边界。这样可避免你只想验证功能时，智能体却更改了实现或执行范围过大的操作。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选的，但填写后会为智能体提供明确的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏任何职责或边界，请先要求默认智能体修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时载入项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，让它使用已有的议题和规划决定。

1. 使用以下提示词启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能不会立即出现在可用智能体列表中。它仍然存在，上述命令会启用它。

2. 使用以下提示词要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作！
4. 完成后，阅读它提供的报告。

## 总结与后续步骤

你已在工作流中添加一个可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选议题审查了它的发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，你将[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
