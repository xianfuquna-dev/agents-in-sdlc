---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员通常需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件，再次修改……工作流程往往会持续重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以更自主地在 IDE 中工作。它会像开发人员一样先探索项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你互动。

本练习将介绍：

- Copilot 智能体模式如何探索项目、查找相关文件并协同修改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站前，先了解它当前的功能。

该网站是一个带开发者主题的棋盘游戏众筹平台，可列出游戏并显示其详情。网站是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的精简 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择 Codespace 终端中的 localhost URL 会自动将你转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到托管 Web 服务器的 codespace 的私有隧道。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图像和简介。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、简介、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型的选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已可正常运行，但还可以添加新功能。先查看待办事项。你从模板设置项目时，系统已自动创建议题。要求 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此教程列出了该模型，希望建议尽量保持一致。不过，LLM 的输出具有概率性，你可能会收到与教程不同的建议。这完全正常。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令列出所有议题。
5. 查看生成的议题列表。

注意 Copilot 还根据对用户的帮助程度为事项排了优先级。

## 审查指令文件

让智能体生成代码之前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中添加功能时的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径是相对于指令文件所在位置的。这样可以重复使用内容，将复杂指令拆分成较小、易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更改：

- 添加或完善数据层（`src/lib/`）中的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以代为完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示词，让 Copilot 根据相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并找到相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示这两个按钮；工作进行时无需选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直至完成任务且未检测到错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、查找相关文件并协同修改。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习 - 实现分页

游戏列表不断增加后，需要支持分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。可以考虑以下事项：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是固定每页大小。
- 编写提示词时，提供必要指导，确保 Copilot 按照你的设想实现。
- 你可能需要与 GitHub Copilot 反复交互，要求修改并补充上下文。这是正常的工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
