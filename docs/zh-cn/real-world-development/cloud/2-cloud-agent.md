---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。完成后，它会创建待审查的草稿 PR，供开发者检查，从而将繁琐任务交给代理处理，加快开发流程，让开发者专注于更重要的工作。

本练习将探索 Copilot cloud agent 的以下功能：

- 自定义代码生成环境。
- 确保安全执行操作。
- 编写范围明确的 issue。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望每款游戏的详情页显示同类别的其他游戏，方便访问者继续浏览。当前不需要精致的实现，只需要可用于 UX 验收测试的版本。此需求目前会阻塞工作，但优先级低于其他 issue。

这类任务很容易被搁置，因此很适合交给 Copilot cloud agent 异步处理。开发者可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。你可以通过多种方式启动它，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确指令范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考过程。即使看似简单的操作，也可能包含复杂细节，因此要谨慎确定 Copilot cloud agent 的任务范围。将它视为 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何指导 Copilot。如果你已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务前，以只读方式检查本存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或运行中的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` issue 分配给 cloud agent 时，请留意 PR 中的 TSDoc 文档注释、TypeScript 约定和文件注释头；它们来自这些指令文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，它会像软件工程师一样执行工作。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在它开始工作前自定义环境，使它能够访问必要的开发工具和依赖项。实验室已预先配置此工作流，以确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个要点：

- 它包含名为 `copilot-setup-steps` 的单个作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，便于测试，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的文档不是过时，就是缺失。此类技术债务常常得不到处理，降低效率，也让代码库维护和新成员上手变得困难。Copilot 擅长生成文档，因此很适合将这项任务交给 Copilot cloud agent。它会在后台生成所需文档，之后的练习中再审查结果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方法。

13. 查看列表和计划执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定方案，以及生成代码。

这通常需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能处理其他工作。接下来，我们会利用这一点：再分配一个任务给 cloud agent，同时着手为应用编写代码。

## 在游戏详情页添加相关游戏部分

GitHub Copilot cloud agent 的一个重要优势是可以分配并行工作：你处理一项任务，它处理另一项。为设计团队添加相关游戏部分可能不会花很长时间，但仍可将这段时间用于其他工作。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，issue 中的第一条评论旁会显示 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此你不必一直监控；完成后再审查即可。现在可以转向创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 Copilot，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保安全执行操作。
- 编写范围明确的 issue。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以转向创建和使用自定义代理。[Copilot cloud agent 也能使用 MCP 服务器][cloud-agent-mcp]，并可使用自定义指令；我们已在前面的模块中介绍过这些内容。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
