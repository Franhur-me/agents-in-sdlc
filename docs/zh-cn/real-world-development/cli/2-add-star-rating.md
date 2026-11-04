---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查结果，并将更改作为首个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行首次更改了。先从一个小改动入手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示它。你将让代理显示评分、审查更改，并将其作为首个拉取请求合并。

本课将学习如何：

- 在功能分支中开始有针对性的 Copilot 对话。
- 让代理对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认结果。
- 创建并合并首个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，该评分已显示在游戏详情页中。但主页的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让代理在每张卡片上显示现有评分。这项小而独立的更改非常适合首次会话。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，你会看到三类内容：你的提示和代理的回复；代理探索并编辑文件时的工具活动；以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

开始新的对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 开始新会话，然后按 <kbd>Enter</kbd>。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。你已使用 Copilot CLI 添加了新功能！

## 审查差异

即使是很小的 AI 生成更改，也应该在合并前审查。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个更改过的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有概率性，并非每次都相同，因此实际代码可能有所不同。请检查行为，不要期待某种特定实现。

## 检查更改

不要只看代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的界面。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并首个拉取请求

功能已完成，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上开始了有针对性的 Copilot 对话。
- 指示代理对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了首个拉取请求。

接下来，你将从筛选 issue 入手，使用 Plan 和 Autopilot 模式构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
