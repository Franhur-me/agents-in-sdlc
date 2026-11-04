---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

在代理模式下，GitHub Copilot 会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作实时发生在你的计算机上，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或撤销单项更改，并通过后续提示调整或扩展工作，无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍代理的工作。
- 审查并接受代理提出的文件编辑。
- 在会话进行中引导代理，添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行中，设计团队又提出了浅色模式切换的需求，以帮助觉得默认主题太暗的用户。与其从头开始新对话，不如引导现有会话扩展工作范围。

## 监控 Copilot 进度

Copilot Chat 视图会显示代理执行的每个步骤，包括读取的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 代理工作时滚动聊天内容，查看工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 代理提出文件编辑时，差异会内联显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以选择聊天顶部的 **Files changed** 指示器，查看该会话中代理修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式实现。完成时会显示更改摘要。

## 引导代理添加浅色模式

高对比度模式实现后，在同一对话中继续添加浅色模式切换。由于继续使用相同会话，代理会保留之前的上下文，包括修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 代理会检查已做的更改，并继续提出浅色模式相关修改。
3. 像审查高对比度功能一样，逐项检查新更改，并对每个差异选择 **Keep** 或 **Undo**。
4. 如果代理的实现方向不符合预期，例如试图以不希望的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话通常比从头开始更快。可以用后续提示要求调整，例如“简化 CSS，使用 CSS 变量而不是重复选择器”；纠正方向，例如“不要添加第三个切换开关，将浅色模式集成到现有设置面板”；或扩展范围，例如“同时为两个切换开关添加键盘快捷键”。

## 审查工作集

提交工作前，快速检查代理修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个更改过的文件并检查差异。确认其中体现了自定义代理提出的无障碍实践：ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导了一个进行中的会话：逐项审查文件编辑、接受或撤销更改，并在保留上下文的同时发送后续提示扩展工作。

本课探索了以下内容：

- 实时监控代理工作。
- 接受或撤销单项文件编辑。
- 在会话进行中通过后续提示引导代理。

下一课将发布无障碍功能 PR，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
