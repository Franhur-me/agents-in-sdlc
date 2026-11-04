---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中编写 Markdown 文件，可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，使它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理可通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，随后是用于定义代理行为、专长和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]在概念上有些重叠，两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义代理是执行任务的工作者，技能是它使用的工具**。

自定义代理拥有自己的上下文窗口，并用于协调技能，甚至协调其他代理来完成工作。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；它还可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 功能一样，应该通过测试和迭代，找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理时，你将了解：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，当前暗色主题的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启和关闭的高对比度模式。

无障碍访问至关重要，因此你将使用自定义代理尽快生成所需功能。

## 检查无障碍自定义代理

项目已为无障碍任务创建自定义代理。先查看它的内容，了解 Copilot 将如何受到指导。

返回 Codespace，然后检查无障碍代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中定义的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见错误清单。

## 创建并分配 issue

代理控制台是管理环境中所有代理的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，并重新引导或提供额外指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

   ![Copilot cloud agent 分配界面中突出显示自定义代理和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义代理可以将团队的专业知识和标准编写成可复用指南，帮助 Copilot 更有效地完成特定类型的工作。

本课探索了以下内容：

- 如何定义自定义代理。
- 如何分配任务给自定义代理。

Copilot 正在实现高对比度模式。接下来，前往代理控制台[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
