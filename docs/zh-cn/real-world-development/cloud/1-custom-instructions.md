---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 理解你不仅需要它做什么，也需要它如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，这里不会运行更改指令前后的对比提示。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示影响。稍后审查它创建的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 和其他优秀开发团队一样，制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加说明该文件用途的注释块。

通过使用指令文件，你可以确保 Copilot 按照这些实践执行任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，让 Copilot 提供更贴合需求的建议和代码片段。你可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应包括与大多数聊天或 CLI 请求有关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件不在本工作坊范围内。本项目中的示例展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应如何编写或不应如何编写的示例。

指令文件没有唯一的编写方式，AI 的使用也不例外。通过试验，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。浏览项目文件时，你会看到许多不同任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions** 或 Copilot CLI 中的 `/init`。对应界面的课程会在相关处说明。
>
> 如需模板或入门示例，请浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读本存储库提供的指令文件：其中包括一个核心 `copilot-instructions.md` 和多个针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括适用于 Astro、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来限定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看此文件中针对本项目单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动至底部。注意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成可复用的小文件，并提供生成代码时的示例。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来要编辑一次文件：添加项目级规则，要求将说明写在代码中，包括 TSDoc 文档注释和文件级注释头。后续练习中 cloud agent 处理 issue 时会采用这条规则。

编辑前先创建分支（cloud agent 会从 issue 目标分支读取指令，但仍建议干净地提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。注意该部分记录了项目编码标准，但尚未规定代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令既可以放在存储库级 `.github/copilot-instructions.md` 中，也可以放在适用于特定语言、文件类型或任务的 `*.instructions.md` 文件中。存储库级文件适合放置刚添加的文档注释等全局标准。

## 提交、推送并合并指令更新

cloud agent 会读取 issue 目标分支中的指令。在下一练习中将 issue 分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理 issue 时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过创建分支，并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了存储库级标准。在本练习剩余部分中，你会看到 cloud agent 创建的 PR 如何应用此标准。

接下来，将[首个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
