---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者往往需要找出所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并再次修改，工作环节很多。

Copilot Agent Mode（代理模式）正是为此而设计的。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它会像开发者一样先探索项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，适合快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot Agent Mode 探索项目、识别相关文件并协调更改。
- 使用 Copilot Agent Mode 跨 UI 和数据层实现新功能。
- 在将更改合并到代码库之前审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解当前功能。

网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详情。它是一个单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 frontmatter 和 Drizzle ORM 直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过标准 DOM API 和范围限定的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后，你应该位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中从终端选择 localhost 链接时，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是转发到正在运行网站的 Codespace 的私有通道。

### 浏览网站

网站启动后，可以浏览现有功能：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后，会打开显示标题、描述、发行商和类别等详细信息的页面。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

网站的初始实现已经可用，现在要添加新功能。先查看待办列表。使用模板创建项目时，系统已自动创建 issue；让 GitHub Copilot 为你列出这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此选用该模型以便让代码建议相对一致，保证练习体验。但由于 LLM 具有概率性，你得到的建议可能与示例不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看 issue 待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

注意 Copilot 还根据对用户的帮助程度对事项进行了优先级排序。

## 审查指令文件

让代理生成代码前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [user interface (UI)][ui-instructions] 文件，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令、将大型指令集拆分成较小部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选，至少需要更新应用中的三个部分：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot Agent Mode 可以帮你完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图代理下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败，这没有问题。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot Agent Mode 会话完成示例](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了新功能。你了解了：

- 如何使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- 如何在将代码合并到代码库之前审查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 奖励探索练习 - 实现分页

随着游戏列表不断增大，需要启用分页。使用本练习中学到的技能，让 Copilot 更新网站并实现分页。提示时请考虑以下事项：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页的实现方式，例如是否允许用户选择页面大小，或将其设为固定值。
- 编写提示时，提供 Copilot 所需的指导，帮助它按你的期望实现功能。
- 你可能需要与 GitHub Copilot 多轮迭代、提出更改并补充上下文。这是正常的协作方式。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
