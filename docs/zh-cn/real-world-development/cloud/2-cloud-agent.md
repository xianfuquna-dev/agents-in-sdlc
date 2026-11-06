---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都在处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因没有时间实现而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这让团队可以将繁琐工作交给智能体，加快开发进度，并把精力放在更重要的事项上。

你将使用 Copilot 云智能体了解以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有为代码添加文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示相关游戏（即同一类别中的其他游戏），方便用户继续浏览。目前不要求实现效果完善的功能，只需能用于验收测试用户体验。此功能暂时受阻，而还有其他优先级更高的议题需要处理。

这些任务经常会被搁置，很适合分配给 Copilot 云智能体。它可以异步处理任务，开发人员则可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度，设置环境并开始处理。你可以在会话进行中或完成后查看它的工作。方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 指令范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，也可能隐藏复杂问题，因此要[谨慎限定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。使用生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看此存储库中的指令文件，稍后便能观察其影响。

在 GitHub Web 界面或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求如何遵循这些指令。稍后审查 PR 时，我们还会再次讨论这些内容。

## 设置 Copilot 云智能体的开发环境

不论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也一样，云智能体会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，自定义它开始工作前运行的步骤，以便提供所需开发工具和依赖项。实验已预先配置此工作流，为后续学习做好准备。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用的迁移和填充后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为 *你的步骤* 设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点值得注意：

- 文件只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务经常得不到处理，降低开发效率，也使维护代码库或引入新团队成员更加困难。Copilot 擅长创建文档，因此很适合把这项工作分配给 Copilot 云智能体。它会在后台生成文档，稍后的练习中你将回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定处理问题的方法。

13. 查看 Copilot 准备完成的事项。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并制定计划，然后编写代码。

整个过程可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们能腾出精力处理其他工作。接下来，我们会利用这一点：先分配另一项任务给云智能体，再转而为应用添加功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优势是可以分工处理任务，让你专注于一组工作，而智能体处理另一组工作。为设计团队添加相关游戏区域可能不会花很长时间，但这段时间仍可用于其他任务。现在将其分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此你不需要主动监控，而是在完成后审查结果。现在继续了解自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体，你可以将议题分配给 Copilot 异步处理。它可以帮助解决技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务。接下来，我们将了解如何创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
