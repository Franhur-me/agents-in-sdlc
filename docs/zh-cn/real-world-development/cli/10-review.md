---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预先创建的筛选 issue。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了首个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令指导代理，然后自定义现有 `quality-checks` 技能，用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用真实浏览器体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、分享和可选云端委派的斜杠命令。

## 你交付的内容

本工作坊有两个 PR 阶段，每个都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片中显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化流程，我们将所有工作集中在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖率审查结合起来，再交由 PR 审查。

新增测试应解决实际缺口；如果 QA 检查后认为不需要新增测试，也可能是合理结果。授权合并前，请审查代码和证据；代码更改后，应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可以在其他对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专家角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进聚焦的更改，使用 **Autopilot** 处理范围明确的任务。例行编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文仍与基础设施同样重要。清楚说明要构建什么、为什么构建以及如何构建，会显著影响输出。

## 更多可探索内容

核心工作流已经完成。还可以了解一些 Copilot CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务交给 Copilot cloud agent。

## 后续步骤

持续使用工具是提升熟练度的最佳方式！用它编写生产代码、兴趣项目，或实现你多年来一直想做的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端继续扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
