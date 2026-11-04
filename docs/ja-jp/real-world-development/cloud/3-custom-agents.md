---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI assistant を作成できます。repository の `.github/agents` folder に Markdown file で agent を定義し、特定の作業を効果的に行うための instruction、ベスト プラクティス、コーディング パターン、専門知識を指定します。チームが持つ知識を再利用可能な agent としてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディング プラクティスに従う security agent、一貫した test pattern を保つ testing agent などです。

Custom agent はプロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には少なくとも `name` と `description` を含む YAML frontmatter があり、その後にエージェントの動作、専門性、instruction を記述します。

### Custom agent と agent skill の違い

Custom agent と [agent skill][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown file で定義され、AI に作業方法を伝えます。分かりやすく言うと、**custom agent は作業者、skill は tool**です。

Custom agent は独自の context window を持ち、skill や他の agent を連携させて作業できます。この演習では、アクセシビリティ custom agent がガイドラインに沿ってサイトをレビューし、更新します。その過程で、pull request workflow skill や test 実行用 skill を呼び出す場合があります。

> [!NOTE]
> Custom agent に唯一の正しい書き方はありません。AI の他の活用と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次のことを学びます。

- custom agent の定義方法。
- custom agent へのタスク割り当て。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームにアクセスできるよう取り組んでいます。現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックが寄せられています。デザイン チームは、利用者が切り替えられる高コントラスト モードを求めています。

アクセシビリティに関するこの問題をすばやく解決するため、custom agent に機能の生成を依頼します。

## アクセシビリティ custom agent を確認する

アクセシビリティ向けの custom agent はすでに用意されています。内容を読み、Copilot に与えるガイダンスを確認します。

Codespace に戻り、次のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき実装。

## Issue を作成して割り当てる

Mission control は、環境内の agent を管理する場所です。Copilot cloud agent に作業を割り当て、進捗を監視し、追加のガイダンスを与えられます。高コントラスト モードの作成を Copilot に依頼しましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択します。
9. 一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で custom agent と accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[GitHub Copilot の custom agent][custom-agents]は、特定の作業や分野に特化した AI assistant です。custom agent を使うと、チームの知識や標準を再利用可能な形でまとめられます。

- custom agent の定義方法を学びました。
- custom agent にタスクを割り当てました。

Copilot が高コントラスト モードを実装している間に、[mission control から agent session を監視して案内します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使う準備][org-custom-agents]
- [Enterprise で custom agent を使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
