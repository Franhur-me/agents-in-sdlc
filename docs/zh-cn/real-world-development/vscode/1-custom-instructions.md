---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的一种主要方式。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选代码切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不符合所有自定义标准，因为 Copilot 的结果具有概率性。本练习的目标是观察更新指令前后行为的变化趋势，而不是让输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 和其他优秀开发团队一样，制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加说明该文件用途的注释块。

通过使用指令文件，你可以确保 Copilot 按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，让 Copilot 提供更贴合需求的建议和代码片段。你可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应包括与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应如何编写或不应如何编写的示例。

指令文件没有唯一的正确写法。通过试验，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。项目中包含适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions** 或 Copilot CLI 中的 `/init`。对应界面的课程会在相关处说明。
>
> 如需模板或入门示例，请浏览 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

先阅读存储库提供的核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件，可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。
3. 打开 `.github/instructions` 文件夹并查看其中的指令，包括 Astro、Drizzle 数据层和测试等。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 限定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动至底部。查看指向其他指令文件和项目现有文件的链接。相对于指令文件的路径可帮助 Copilot 使用示例，也让指令集更容易拆分和复用。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一个练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

先使用当前指令发送提示，观察其影响；稍后更新指令后，再次发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获得不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 能应用文件更改。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它可能会在没有明确指令的情况下添加文档注释。如果发生这种情况也没关系，本练习的重点是观察更新指令后行为是否更加一致。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。注意当前规则尚未要求添加代码文档。
3. 在现有标准下方添加以下 Markdown 内容，要求添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令既可以放在存储库级 `.github/copilot-instructions.md` 中，也可以放在适用于特定语言、文件类型或任务的 `*.instructions.md` 文件中。存储库级文件适合放置刚添加的全局标准。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在也包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续构建。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用版本控制。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办列表中关于更新指令文件的 issue。你可以手动操作，也可以让 Copilot 使用已连接到项目的 GitHub 工具。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用时，VS Code 可能会提示你登录 GitHub。请按提示完成授权。后续练习会介绍这种连接所使用的 Model Context Protocol。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保代理选择器中已选择 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 当 Copilot 提示允许它代表你执行 GitHub 操作时，检查请求并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并让本地分支回到 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示允许执行 GitHub 操作和 shell 命令时，检查请求并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，观察 Copilot 在*现有*指令下生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
