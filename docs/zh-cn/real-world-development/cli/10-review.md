---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已通过 GitHub Copilot CLI 完成 Tailspin Toys 的连续工作流程。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目并找到预先创建的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中制定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，均从已更新的 `main` 分支创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新指令并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建其 PR，你一直使用同一个对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中体验功能。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 则在 PR 创建前综合检查需求和测试覆盖情况。

新增测试应解决实际缺口；如果 QA 检查后不需要增加测试，也可能是合理结果。授权合并前，请审查代码和证据；更改代码后，请重新生成受影响的证据。

## 最佳实践

你为 Copilot 提供的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并设置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 在开始构建前思考方案，使用 **Interactive** 处理需要密切参与的更改，并使用 **Autopilot** 处理范围明确的任务。例行编辑可选择较快的模型，复杂任务则可选择能力更强的模型。

上下文和基础设施同样重要。明确描述希望构建的内容、原因和方式，会显著影响生成结果。

## 继续探索

你已完成核心工作流程。以下 Copilot CLI 功能也值得一试：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立的子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是持续使用它！可以用它处理生产代码、业余项目，也可以尝试实现多年来一直想做却尚未着手的小应用。与团队分享经验，也向团队成员学习。最后，别忘了继续探索文档。

若要从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
