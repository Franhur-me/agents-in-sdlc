---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、分享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、查看内部信息和配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或分享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他功能：管理上下文、切换模型、分享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和可用空间的上下文分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不大。

> [!NOTE]
> Copilot CLI 会在上下文窗口接近上限时自动压缩内容。若要自行选择时机，请使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有各自偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 分享会话

团队合作和分享经验有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可分享链接，或发布 GitHub gist。

1. 输入 `/help`，查看你所用版本中 `/share` 的选项。
2. 如果要分享此会话，请选择合适的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 发送或发布导出内容前，请先检查内容。会话导出可能包含提示、回复和项目细节。

发布链接或 gist 是可选操作。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已完成。Copilot CLI 还为较大型的任务提供了以下命令：

- `/fleet` 可将独立子任务分配给子代理并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务交给 Copilot cloud agent 异步处理，并可能创建拉取请求。

这些命令可能会创建其他工作树或触发远程工作，因此属于可选内容。尝试前，请先开始一个范围明确的新任务，并通过常规工作流审查结果。若要深入了解异步代理工作，可以继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可进行配置、分享会话并查看内部状态。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或分享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！通过[回顾已学内容][next-lesson]和后续学习计划，结束本次学习。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
