---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要完成一些准备工作。你将创建 Tailspin Toys 存储库的副本，并启动一个 [Codespace][codespaces]，用于编辑指令文件和审查 cloud agent 生成的工作。

## 设置实验室存储库

你将从[模板存储库][template-repository]创建实例，作为编写代码的副本。新实例包含实验所需文件，可在练习中使用。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后练习中会用到。

## 创建 Codespace

接下来，使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它是一个功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但比手动安装所有服务快得多。等待期间，你可以先了解 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会返回此 Codespace。现在请让它保持打开状态，并保留在浏览器选项卡中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行，两者都包含顺利完成练习所需的先决条件。若要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建实验室存储库的副本，并开始创建 Codespace。后续练习中，你将在该环境中与 Copilot cloud agent 协作。

## 下一步

接下来，添加 cloud agent 要遵循的自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
