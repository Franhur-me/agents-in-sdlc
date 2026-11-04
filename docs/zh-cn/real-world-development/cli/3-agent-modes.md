---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助团队以正确方式构建正确功能。本系列课程将体验典型的代理式开发流程：从 issue 着手实现新功能，确保代码有效、行为符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能流程会继续使用同一对话和分支。实际工作中，通常会针对不同文件类型使用不同分支或 PR；本教程为聚焦核心概念而简化了流程。

本课将学习如何：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

完成后续功能时，你还将更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办 issue 描述了该功能，但例如是否允许组合多个类别等细节需要先达成一致。你将使用 Plan 模式确定这些决策，再授权 Autopilot 在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理，并不会改变软件开发的基本原则；这些原则反而更加重要。多数开发者的流程大致如下：

1. 从描述需求的 issue 开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作；代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先使用 Plan 模式审查计划，然后切换到 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能之前，返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
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
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求（PR）加入上下文。

## 规划筛选功能

规划能让你在开始实现功能或执行任务前确定方案。对于复杂工作，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方式，制定计划。

3. Copilot 可能会询问筛选功能的具体行为。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划撰写并审查完毕后，就可以开始实现了。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 反复处理问题，直至它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中含义相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。现在一起审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能的实现和测试。
3. 根据 issue 和规划时作出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但是否真的能运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub issue 开始了新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，深入了解代码生成方式，使用[自定义指令][next-lesson]确保代码符合文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
