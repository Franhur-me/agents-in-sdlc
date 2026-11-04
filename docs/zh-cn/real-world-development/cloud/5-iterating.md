---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。现在来检查代码更改；必要时向 Copilot 提供反馈，改进其工作，然后再允许团队审查。

### 场景

正如前面多次强调的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在开启团队审查之前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提供的更改。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 任何 GitHub Actions 工作流都必须经过人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR）：为代码添加文档。通过 GitHub.com 上常规的 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。你可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改会有所不同。

   检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理 PR 并非单向过程。你可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。结果具有非确定性，因此无法提供唯一的提示文本。你可以要求 Copilot：

- 在每个代码文件开头添加简短的文件注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在评论中使用 **@copilot** 提出文档更改请求。可以使用上面的任一建议，也可以提出其他文档改进请求。
2. 选择 **View Session** 观察 Copilot 执行更改。注意 Copilot 会启动新会话。
3. 可以选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，并合并到 main 分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 或描述更完善的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中检出此分支，手动测试相关游戏功能。打开终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，你可以通过评论迭代 cloud agent 的工作，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（注意：此 issue 在自定义指令更新前就已分配）。在 **Conversation** 选项卡添加评论，Copilot 就会收到并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在代理控制台提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，重点检查：
   - 切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**在 Codespace 中检出此分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置能在重新加载页面后保留。

注意，无障碍自定义代理帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理其他 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 — 继续委派工作

Cloud agent 最适合处理待办列表中的真实任务，让你可以继续做其他事情。可以在自己的存储库中创建更多 issue 并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页添加支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 练习。在这些课程中，你：

- **检查了存储库提供的自定义指令文件**，了解它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**处理无障碍任务，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制台**，监控并引导无障碍会话。
- **审查并迭代 cloud agent 创建的拉取请求**，使用 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

Cloud agent 练习已经完成。若要继续探索，可查看其他练习：

- 🖥️ **[VS Code 练习](../../vscode/)** — 在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)** — 从终端交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、代理技能、QA 自定义代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 功能。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
