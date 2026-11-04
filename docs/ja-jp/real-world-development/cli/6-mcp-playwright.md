---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定して、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot が利用できる自動化を追加したりする必要があります。MCP server を使うと、Copilot CLI に組み込まれていない tool やサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を理解します。
- まだ利用できない場合は Playwright MCP server を追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

unit test と end-to-end test は重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにし、期待どおりに動作するか確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部の tool やサービスと通信するための仕組みです。MCP を使うと、AI agent は最新情報にアクセスし、ユーザーに代わって操作できます。

tool と resource は、AI agent と外部サービスをつなぐ MCP server を通じて利用します。各 MCP server は、エージェントが利用できる tool と resource のセットを提供します。

よく使われる MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub repository、issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP server が公開されています。GitHub は、発見や貢献を促進するための [MCP registry][mcp-registry] を運営しています。

> [!CAUTION]
> MCP server も、他の依存関係と同様に扱ってください。利用前にソース コードをレビューし、公開元を確認して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

まだ Playwright MCP server が利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定済みの場合は、セットアップを省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次の command を入力し、MCP server 追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、もう一度 <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP 画面を閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおり動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP server を使います。

1. 次のプロンプトを使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は指示しなくても MCP server を見つけられます。どの server を使うか分かっている場合は、指定すると結果が安定し、token も節約できます。

2. Copilot がブラウザーを操作して機能を確認する様子を見ます。
3. 結果を読み、すべてが期待どおり動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作してから server を停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使って、実際のブラウザーで機能を確認しました。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を学びました。
- 未設定の場合に Playwright MCP server を追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、skill とブラウザー tool を組み合わせる[QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
