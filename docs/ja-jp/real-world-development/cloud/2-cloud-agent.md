---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が終わると、人間の開発者がレビューできる draft PR を作成します。手間のかかる作業を任せて開発を加速し、より重要な作業に集中できます。

この演習では Copilot cloud agent を使って、次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確にした issue の重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys は技術的負債に対処しようとしています。最初のサイトを開発した委託先がドキュメントを残していないため、まずアプリケーションのすべての export 関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲーム詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示します。完成された実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い issue がほかにあります。

このように優先度が下がりやすい作業は、Copilot cloud agent に割り当てるのに適しています。非同期で作業させれば、他のタスクに集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を記録する draft pull request を作成し、環境を準備して作業を開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。簡単そうに見える操作でも、詳しく調べると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI pair programmer として扱い、段階的に作業し、学びながら試して調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

これまでの演習で custom instruction と Copilot への影響を学びました。まだ `.github/copilot-instructions.md` を読んでいない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction file を確認します。

- `.github/copilot-instructions.md` — TypeScript の規約と TSDoc doc comment に関する **Code standards** を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` に instruction を適用することを確認します。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てます。生成された PR で TSDoc doc comment、TypeScript 規約、コメント ヘッダーを確認してください。これらの内容は instruction file によるものです。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、必要な開発 tool と依存関係を整えるための環境設定が必要な場合があります。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用の setup workflow][setup-workflow]で、必要な開発 tool や依存関係を用意できます。この演習では、単一 Astro app に必要な Node.js、プロジェクト依存関係、end-to-end test 用ブラウザー、migration 済みで seed 済みの SQLite database がすでに設定されています。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一の job があります。この job は Copilot が PR の作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うと、リポジトリの **Actions** タブから手動で workflow を実行できます。Copilot が実行するのを待たずに、workflow が成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり不足していたりするプロジェクトは少なくありません。このような技術的負債は後回しにされ、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成も得意です。最初の issue として割り当て、後の演習で成果を確認しましょう。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当て画面を開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかる場合があります。Copilot が環境を作り（前述の workflow を実行し）、プロジェクトを分析して、作業方法を検討しています。

13. todo list と、実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot がコードを調査して状態を把握し、計画を考えてコードを作成する様子を確認します。

処理には数分かかることがあります。Copilot cloud agent の主な目的は、非同期で作業している間に別のタスクを進められることです。もう 1 つの作業を cloud agent に割り当て、その間にアプリの機能開発に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けに関連ゲームを追加する作業を任せ、その間に別のタスクを進めましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択します。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

間もなく、Copilot が作業を始めたことを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue を処理中であることを示す目の emoji](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。ソフトウェア エンジニアのように作業するので、常に監視する必要はありません。完了後に結果をレビューしましょう。次は custom agent の作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、非同期で issue に取り組むよう Copilot に依頼できることを学びました。技術的負債への対応、新機能の作成、framework の移行などに利用できます。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした issue の重要性を学びました。
- issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、[custom agent の作成と使用][next-lesson]に進みます。[Copilot cloud agent は MCP server][cloud-agent-mcp]と custom instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
