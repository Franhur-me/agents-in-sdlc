---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

agent mode で絞り込み機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くか確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際の browser を操作してテストしてもらいます。その後 branch を公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP server が新しい tool で Copilot を拡張する方法を学びます。
- workspace に Playwright MCP server を追加します。
- Copilot に絞り込み機能をブラウザーで手動テストしてもらいます。
- branch を公開し、絞り込みの PR を作成します。

## Model Context Protocol (MCP) とは

agent mode は editor の外にある tool へアクセスできると、さらに便利になります。Copilot が外部の tool や service と通信する標準的な方法が Model Context Protocol (MCP) です。

![agent mode の内部のしくみと、context、LLM、MCP server や VS Code extension が提供する tool との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は AI agent が外部の tool や service と通信する方法を提供します。resource を使って最新情報にアクセスしたり、tool を使ってユーザーに代わって操作したりできます。

tool と resource は、AI agent と外部の tool・service をつなぐ MCP server を通じて利用します。MCP server は既存 API や npm package などのローカル tool との通信を管理します。各 server は、エージェントが利用できる独自の tool と resource を提供します。

よく使われる MCP server の例です。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API にアクセスし、新しい repository の作成、既存 repository の更新、issue や pull request の管理を行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright による browser automation を提供し、Web page の移動、form 入力、button 選択などを実行できます。

さまざまな tool や resource を提供する MCP server がほかにもあります。GitHub は、発見や貢献を促進する [MCP registry](https://github.com/mcp) を運営しています。

> [!CAUTION]
> セキュリティの観点から、MCP server も他の依存関係と同様に扱ってください。利用前にソース コードを注意深くレビューし、公開元を確認して、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密 resource や操作へのアクセスを許可する際は慎重に判断してください。

## MCP の設定を確認する

`.vscode/mcp.json` file で、この VS Code workspace で利用可能な MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` server が設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` entry は project template に含まれているため、前の演習で Copilot は issue backlog を読み取れました。[remote GitHub MCP server][remote-github-mcp-server]を使っており、ローカル インストールは不要です。VS Code は HTTP 経由で接続し、Copilot が最初に tool を使う際に GitHub で認証します。

## Playwright MCP server を追加する

2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が browser を操作できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` entry を追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. file を保存します。

remote の `github` server と異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag は browser window を表示せずに実行します。desktop がない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys project では end-to-end test に Playwright を使っているため、通常 browser はインストール済みです。Copilot から browser がないと報告された場合は `npx playwright install chromium` を実行するよう依頼し、再試行します。

## Server を起動して信頼する

VS Code は MCP server を**必要に応じて**起動します。Copilot が初めて server の tool を使う際に、VS Code が起動して信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上にある **Start** action を選択し、server を起動して設定を確認します。
2. VS Code から server を信頼するか確認されたら、設定を確認し、起動を許可します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が tool を使うときに VS Code が起動し、信頼の確認を表示します。

## 絞り込み機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際の browser を操作できます。アプリを自分で操作する代わりに、agent が page を開き、移動し、filter を適用して結果を読み取り、要約します。会話を離れずに機能を確かめる方法です。

Playwright MCP server は screenshot ではなく、page の [accessibility tree][playwright-mcp-server] を使います。そのため、agent は button、link、list item など、支援技術と同じように構造化された label 付き element を確認します。簡単な機能確認がアクセシビリティの基本確認にもなります。

server が接続され、app が起動している状態で、次の prompt を Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由で browser を起動し、手順に沿って操作して結果を報告します。issue の受け入れ条件に照らして確認してください。問題があれば追加の質問をするか、PR 作成前に修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` で app が起動している必要があります。dev server を停止している場合は prompt を送る前に再起動します。Playwright MCP server の初回使用時に browser をダウンロードする場合があります。browser がないと報告された場合は `npx playwright install chromium` を実行するよう依頼して、再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Branch を公開して pull request を作成する

機能の動作を確認したので、チームのレビュー用に PR を作成します。まず `filtering-vscode` branch を公開します。

1. Codespace の **Source Control** panel を開き、Copilot が行った変更を確認します。
2. **+** icon を選択して変更を stage します。
3. **Sparkle** button を選択して commit message を生成します。

   ![変更内容が表示された Source Control panel](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択し、repository に branch を push します。

## Pull request を作成する

GitHub.com や GitHub command-line interface (CLI) などから PR を作成できます。ここでは Copilot に関連 issue を検索し、紐付けた PR を作成してもらいます。

1. Copilot Chat panel に移動し、**New Chat** を選択します。
2. GitHub tool を使えるよう、agent dropdown で **Agent** が選択されていることを確認します。
3. 次の prompt で PR 作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作するため **Continue** を選択します。GitHub tool の初回利用時に認証を求められたら、案内に従います。
5. Copilot が issue を検索して該当のものを見つけ、PR を作成したことを確認します。
6. Copilot が表示した link を選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

- Model Context Protocol (MCP) と MCP server が Copilot を拡張する方法を学びました。
- workspace に Playwright MCP server を追加しました。
- feature を公開する前に browser 上でテストしました。
- branch を公開し、絞り込みの PR を作成しました。

MCP server を使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどに使う[custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
