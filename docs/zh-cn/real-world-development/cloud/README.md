---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理工作。你在 GitHub 上分配任务后，cloud agent 会在后台探索存储库、进行更改并创建拉取请求，让你可以继续处理其他事情。

在这些练习中，你将添加 cloud agent 要遵循的自定义指令，然后分配 GitHub issue，让它实现相应工作。你会审查并使用自定义代理来指导其工作方式，在代理仪表板中监控并引导会话，最后审查代理创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 要遵循的自定义指令 |
| [2. Cloud Agent][ex2] | 异步代理 | 将 issue 分配给 Copilot cloud agent |
| [3. 自定义代理][ex3] | 专项代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确认你具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的代理、MCP、代码审查和 Copilot CLI 功能，因此可完成全部练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用 cloud agent，只有 Copilot Free 计划除外。对于 Copilot Business 或 Enterprise，管理员必须为组织启用该功能。

> [!NOTE]
> 本练习不涉及 MCP。若想了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 练习][cli-harness]或 [VS Code 练习][vscode-harness]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
