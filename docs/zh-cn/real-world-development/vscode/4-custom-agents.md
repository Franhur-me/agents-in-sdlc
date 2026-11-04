---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中编写 Markdown 文件，可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理可通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，随后是定义代理行为、专长和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]在概念上有些重叠，两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义代理是执行任务的工作者，技能是它使用的工具**。

自定义代理拥有自己的上下文窗口，并用于协调技能，甚至协调其他代理来完成工作。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 功能一样，应该通过测试和迭代，找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，当前暗色主题的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启和关闭的高对比度模式。

无障碍访问至关重要，因此你将使用自定义代理尽快生成所需功能。

使用自定义代理时，你将学习：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 检查无障碍自定义代理

项目已为无障碍任务创建自定义代理。先查看它的内容，了解 Copilot 将如何受到指导。

返回 Codespace，在终端中从 `main` 分支创建新的无障碍功能分支（让练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中定义的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见错误清单。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，聊天会话将使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

1. 如果代理下拉菜单中尚未选择 **Agent**，请选择它。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认情况下会显示当前代理 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审查。
6. 随着编辑应用，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义代理可以将团队的专业知识和标准编写成可复用指南，帮助 Copilot 更有效地完成特定类型的工作。

本课探索了以下内容：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [Awesome Copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
