---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未可用时添加它。
- 让代理操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要实际与界面交互。你可以让 Copilot 使用正在开发的网站，就像用户一样操作，从而进一步自动化更改流程，并增强对功能行为的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问外部工具和服务，获取最新信息并代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具、服务之间的桥梁，每个服务器都代表代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，让 Copilot 能像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你手动确认了功能行为。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，因此不必特意指定。不过，明确指出所需工具有助于获得一致结果，也能节省一些 token。

2. 观察 Copilot 列出它在浏览器中执行的各项操作。
3. 阅读报告，确认一切行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器，从 Copilot CLI 中通过真实浏览器体验了功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚不可用时添加了它。
- 让代理操作浏览器并体验筛选功能。

接下来，你将[创建 QA 自定义代理][next-lesson]，结合技能和浏览器工具，构建专门的质量保证角色。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
