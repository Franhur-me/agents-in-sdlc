---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単なアプリ更新でも、通常は複数の file の変更や test の実行が必要です。開発者は必要な file を見つけ、変更し、test を実行して、見落とした file を調べて、また変更するという作業を繰り返します。

Copilot Agent Mode は、IDE 内でより自律的に作業するための機能です。開発者のようにプロジェクト構成を調べ、必要な変更を行い、test などの task を実行して、問題が見つかれば修正します。Agent Mode を使って、Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。Agent mode は IDE 内で動作するため、すばやいフィードバックとやり取りが可能です。Cloud agent は issue と pull request を介して連携する、非同期のチームメンバーのように動作します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定して変更を調整する方法。
- UI とデータ レイヤーにわたって機能を実装する方法。
- Copilot が生成した変更と test をレビューしてから codebase にマージする方法。

## シナリオ

game の一覧が大きくなってきたため、カテゴリや発行元で絞り込めるようにします。前の演習では publishers helper を追加しました。次は Copilot Agent Mode を使って、残りのデータ レイヤー、UI、test の作業を行います。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の動作を確認します。サイトは開発者をテーマにしたボードゲームのクラウドファンディング platform です。game 一覧と詳細を表示する単一の Astro app で、build 時に page を静的 HTML として render します。page の frontmatter から Drizzle ORM を通じてローカル SQLite database を直接 query します。別の backend API や client-side UI framework はありません。再利用可能な data-access helper は `src/lib/` に置き、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` task で単一の Astro dev server を起動します。

1. Codespace に戻ります。前の演習後なので `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）で Codespace terminal を開きます。
3. 絞り込み用の branch を作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次の command で Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開く prompt が表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、terminal に表示された `http://localhost:4321` を選択します。

> [!NOTE]
> Codespace terminal で localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。Codespace で起動した Web server への private tunnel です。

### Web サイトを確認する

Web サイトを起動したら、次の主な機能を確認します。

- **Home Page**: game のタイトル、画像、説明を一覧表示します。
- **Game Details Page**: game を選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model は演習ごとに異なるため、該当の手順で説明します。

template から作成した project には issue のバックログが用意されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat view の agent dropdown で **Agent** を選択します。Agent は複数 file にわたる変更の計画と実装、terminal command の実行、tool の呼び出しを自律的に行います。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. 利用可能な model の一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> この model の選択は、著者が特定の model を推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使い、比較的一貫した体験になることを期待して記載しています。ただし LLM は確率的に動作するため、提案は異なる場合があります。これは想定された動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP command を使う場合がありますが、タスクを完了できるはずです。

3. 次の prompt で repository の issue backlog を尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して全 issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有益だと判断した順に issue を優先順位付けしたことを確認します。

## Instruction file を確認する

コード生成を開始する前に、Copilot にコンテキストを渡す instruction file を確認します。この演習では、Web サイトの機能追加方法を説明する [user interface (UI)][ui-instructions] file を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。
   - architecture の概要。
   - component 設計、testability、accessibility の原則。
   - Astro、Tailwind CSS、Drizzle などの file 向け instruction への link。

> [!TIP]
> Instruction file から、他の instruction file やプロジェクト内 file を参照できます。path は instruction file の場所からの相対 path です。これにより大きな指示を小さく再利用しやすく分け、例やひな形を参照できます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) の絞り込みロジックを追加または調整する。
- 絞り込み動作の test を追加または更新する。
- game 一覧 page に絞り込み UI を追加する。

すべての test を実行して成功させてから merge します。Copilot Agent Mode に依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** で新しく始めます。
2. **Add Context**、**Instructions**、**ui** を選び、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat view で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. model が引き続き **Claude Sonnet 4.5** であることを確認します。
5. backlog の関連 issue に基づいて機能を実装するよう、次の prompt を送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot が project を調査し、data-layer helper、UI、test file を見つけて変更し、test を実行する様子を確認します。

   ![Copilot が project file を調査している](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は project の調査、file の変更、test の実行を行います。task や codebase の規模によって数分かかることがあります。作業中、editor に **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はなく、完了後にすべての変更について選べます。

7. Copilot に促されたら **Continue** を選択して test を実行します。

   ![Copilot Chat で test の実行許可を求める dialog](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり test が失敗したりすることがあります。Copilot は、エラーがないと判断するまでコード生成と test を繰り返します。

   ![Copilot Agent Mode の完了した Chat session](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを開いている browser に戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら Copilot Chat window で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加しました。

- Agent Mode が UI とデータ レイヤーにわたる機能を実装する方法。
- project を調査し、関連 file を特定して変更を調整する方法。
- 生成された変更と test をレビューしてから merge する方法。

次は [Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加演習 — ページ分割を実装する

game 一覧が大きくなればページ分割が必要になります。この演習で学んだ方法を使い、Copilot に pagination を依頼してください。

- 既存の instruction file を使うなど、project のベスト プラクティスに従います。
- page size を利用者が選択するのか、固定にするのか決めます。
- 望む実装に必要なガイダンスを prompt に含めます。
- 必要に応じて Copilot に変更や追加 context を伝えながら反復します。これは Copilot を使う通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
