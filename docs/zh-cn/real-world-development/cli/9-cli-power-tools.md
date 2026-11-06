---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供许多可用于交互的斜杠命令。这些命令提供高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，现在来了解其他一些实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流程已经完成。现在来了解其他功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 浏览 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会遇到模型上下文窗口的上限。Copilot CLI 会在必要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及系统指令、工具、消息和剩余空间之间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。若要自行决定时机，请使用 `/compact`。切换到无关任务并需要全新对话时，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验可以帮助大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 若要共享会话，请选择适合的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 发送或发布导出内容前，请先检查内容。会话导出可能包含提示词、回复和项目细节。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已经结束。Copilot CLI 还为较大型的任务提供了以下命令：

- `/fleet` 可将相互独立的子任务分配给多个子智能体并行执行。
- `/worktree` 可为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可将任务交给 Copilot 云智能体异步处理，它可能会创建拉取请求。

这些命令可能会创建额外工作树或远程任务，因此均为可选。尝试前，请开始一项范围明确的新任务，并通过常规工作流程审查结果。若要深入了解异步智能体工作方式，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

Copilot CLI 斜杠命令可用于配置 CLI、共享会话，以及查看幕后运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来，[回顾所学内容][next-lesson]，并了解后续学习建议。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
