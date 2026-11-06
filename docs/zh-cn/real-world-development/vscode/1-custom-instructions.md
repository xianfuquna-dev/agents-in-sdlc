---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将：

- 了解如何通过存储库自定义指令和按路径限定范围的指令文件，为 Copilot 提供项目上下文、编码指南和文档规范；
- 在现有指令下生成第一个筛选数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加适用于整个存储库的新标准；
- 再次运行相同提示词，观察生成的代码如何遵循新标准；
- 将指令更新和筛选数据切片提交到 `main`，以便 Copilot 在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不会遵循你设置的所有标准。Copilot 的输出具有概率性。本练习的目的是观察更新指令前后行为变化的*趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

像所有优秀的软件开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。借助此功能，你可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至是注释风格。你可以为整个存储库创建指令，也可以为特定文件类型创建指令，为具体任务提供上下文。

指令文件分为两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随针对该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可用于特定任务或文件类型，例如为 TypeScript 或 Astro 提供语言指导，或者为创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他规范中的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应采用和避免的写法。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览此项目中的文件时，你可能会发现其中包含多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。对应产品的课程会在相关处说明。
>
> 想要查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中有许多指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：其中有一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意这里有关于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定指令适用的文件。例如，匹配 `**/*.test.ts` 的任何 TypeScript 测试文件都会匹配。
5. 留意这些针对本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径是相对于指令文件所在目录的，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来，你将添加关于 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型的选择因练习而异；每个步骤会在相关处说明。

要查看自定义指令的影响，先在当前指令文件下发送提示词。稍后更新指令后，再发送相同提示词并比较效果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这些内容来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它也可能在没有明确指示的情况下添加文档注释。这种情况也没问题；本练习关注的是更新指令后行为的一致性改进。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们将在项目编码标准中添加文档要求，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了项目编码标准，但尚未规定在代码中添加文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件，以提供全局指导；也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级指令文件适合放置项目级标准，例如刚添加的文档注释规则。

## 再次运行提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除缓冲区。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一课将基于此代码继续开发。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要通过源代码管理进行维护。现在将指令和分支提交到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以使用项目中已经连接的 GitHub 工具代为完成：查找议题、创建用于解决该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已配置 GitHub 工具，因此无需设置。如果 Copilot 首次使用这些工具时 VS Code 提示你登录 GitHub，请按照提示操作。稍后课程会介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 系统提示你允许 Copilot 代表你执行 GitHub 操作时，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示词要求 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示你允许 Copilot 执行 GitHub 操作和 shell 命令时，请检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你已了解 Copilot 如何从本项目的指令文件中获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示词，观察 Copilot 如何依据*现有*指令生成内容；
- 在 `.github/copilot-instructions.md` 中添加适用于整个存储库的新标准；
- 再次发送相同提示词，观察生成的代码如何遵循新标准；
- 将指令更新和辅助函数基础提交到 `main`，供下一课使用。

接下来，你将在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
