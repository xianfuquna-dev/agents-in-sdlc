---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 能够配合组织现有的工作流程，帮助我们用正确的方法构建正确的内容。接下来的几课将介绍典型的智能体驱动开发流程：从议题着手生成新功能，确保代码有效、功能符合预期，最后成功将其合并到项目中。

> [!NOTE]
> 后续功能开发将继续使用同一对话和分支。通常你会为不同类型的文件创建不同分支或 PR，不过本研讨会会略过这些步骤，让我们专注于核心概念。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了此功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式明确这些决定，然后授权 Autopilot 实现范围明确的功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员都会遵循类似以下的流程：

1. 从已创建的议题开始，其中描述了需要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 根据团队和组织的不同，具体步骤可能有所差异，但大多数流程都与上述做法类似。

遵循这一标准流程，可以确保 AI 生成的代码满足需求，并经历与手写代码相同的验证过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并反复迭代，直至任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace，确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
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
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示词中，并返回 **Session** 选项卡。

请注意，提示词现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划能让你在交给 Copilot 处理前，先确定实现功能或执行任务的方法。对于复杂工作，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

计划已写好并经过审查，现在可以开始实现！使用 Autopilot，让 Copilot 自行完成工作。

Autopilot 会让 Copilot 反复处理问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所安装版本中名称相近的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前必须进行审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题内容及规划期间确定的方案进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否运行？现在像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题要求及你的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[使用自定义指令][next-lesson]，深入了解如何让代码遵循记录在案的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
