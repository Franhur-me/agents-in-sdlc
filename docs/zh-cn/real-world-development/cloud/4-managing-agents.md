---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 处理了三项改进用户体验和添加功能的任务。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制台中，你可以查看所有有未关闭 PR 的代理任务、了解它执行的操作，还可以引导正在进行的会话。

本课将学习如何：

- 使用代理页面监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务交给代理后，团队意识到也应该添加浅色模式。网站样式和切换功能正在更新，因此顺便添加浅色模式很合理。现在，你将引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 注意右侧显示的 PR 面板。你可以在那里查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

现在你已查看活动任务，可以让 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![代理控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始工作。出现类似下方的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![代理页面上的 cloud agent 任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务。它会在完成你发送消息时正在执行的具体步骤后，将新请求加入工作流程。

这同样需要几分钟。等待期间，可以回顾并思考目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪所有活动和已完成的任务、审查代理执行的工作，并调整进行中的任务范围或提供额外指导。

本课探索了以下内容：

- 使用代理控制台和代理页面监控 cloud agent 任务。
- 重新引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，学习如何[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
