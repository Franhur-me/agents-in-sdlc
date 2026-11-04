---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI assistant を作成できます。repository の `.github/agents` folder に Markdown file で agent を定義し、特定の作業を効果的に行うための instruction、ベスト プラクティス、コーディング パターン、専門知識を指定します。チームの知識を再利用可能な agent としてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディング プラクティスに従う security agent、一貫した test pattern を保つ testing agent などです。

Custom agent は project 内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には少なくとも `name` と `description` を含む YAML frontmatter があり、その後に agent の動作、専門性、instruction を記載します。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept]には役割が一部重なるところがあります。どちらも主に Markdown file で定義され、AI に作業方法を伝えます。分かりやすく言うと、**custom agent は作業者、skill は tool**です。

Custom agent は独自の context window を持ち、skill や他の agent を連携させて作業できます。この演習では、アクセシビリティ custom agent がガイドラインに沿ってサイトをレビュー・更新します。その過程で pull request workflow や test 実行用の skill を使う場合があります。

> [!NOTE]
> Custom agent に唯一の正しい書き方はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべての利用者がクラウドファンディング platform にアクセスできるよう取り組んでいます。現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックが寄せられています。デザイン チームは利用者が切り替えられる高コントラスト モードを求めています。

この演習では、次のことを行います。

- 既存の accessibility custom agent を確認します。
- Copilot Chat の accessibility agent を使って高コントラスト モードを実装します。

## Accessibility custom agent を確認する

アクセシビリティ向けの custom agent はすでに用意されています。内容を読み、Copilot に与えるガイダンスを確認します。

Codespace に戻り、アクセシビリティ作業用に `main` から新しい branch を作成します（Exercise 3 の絞り込み PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。
   - アクセシブルな Web サイト向けコード生成の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき実装。

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat view 下部にある agent dropdown には、`.github/agents` に定義されたすべての custom agent が表示されます。custom agent を選ぶと、その instruction と tool を使って chat session を進められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model は演習ごとに異なるため、該当の手順で説明します。

1. Chat view の agent dropdown で **Agent** を選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. Chat view 下部の agent dropdown を選択します。既定では active agent の名前 (**default**) が表示されます。
3. 一覧から **Accessibility agent** を選択します。
4. 次の prompt を accessibility agent に送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat が codebase を調べ、変更を提案して project file に適用します。変更ごとに file path と diff が chat に表示されます。
6. **Files changed** indicator で、この session 中に agent が変更した file を確認します。

> [!NOTE]
> 実際に repository が変更されるため、処理には数分かかることがあります。Astro component、CSS、関連 test などを編集します。

次の演習では、実行中の作業を確認し、指示を追加します。

## まとめと次のステップ

[GitHub Copilot custom agent][custom-agents]は、特定の作業や分野に特化した AI assistant です。custom agent にチームの知識と標準をまとめ、再利用できます。

- custom agent の定義方法を確認しました。
- Copilot Chat の agent mode で custom agent を使用しました。

次は、実行中の agent 作業を[監視して案内します][next-lesson]。変更を確認し、同じ session にライト モードを追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の設定][custom-agents-config]
- [Awesome Copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
