---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーから絞り込みの操作を確認しました。次は、これらの機能を明確な QA プロセスを持つ custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA agent を選択し、絞り込み issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューする必要があります。custom agent で QA プロセスを調整し、再利用可能なレポートを作成できます。

## Custom agent とは

Custom agent は、Markdown プロファイルで定義する Copilot の特化版です。プロファイルにはエージェントの目的、instruction、利用可能な tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instruction はチームの標準を説明し、`quality-checks` skill は繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー tool を提供します。QA プロファイルは、これらの機能を置き換えることなく、また別の会話を必要とせずに、要件の評価と結果の報告にどう活用するかを Copilot に伝えます。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。QA が実施するチェックと、その境界を定義します。

1. Codespace に戻り、絞り込み会話が Interactive mode であることを確認します。
2. 既定のエージェントに次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約を取り込んだことを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須で、`name` は任意ですが、含めると表示名が明確になります。
3. プロファイルの instruction を読み、要件の確認、リポジトリ instruction の参照、`quality-checks` skill の実行、Playwright MCP の利用が含まれていることを確認します。
4. 根拠を添えて報告し、実装コードの変更前に確認を求め、変更を commit したり PR を開いたりしないことを確認します。
5. 責務や制約が足りなければ、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。プロファイル作成後に同じ絞り込み会話を再開し、QA を選択して、issue と計画の決定事項を利用できるようにします。

1. 次の prompt で agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりの agent は一覧に表示されないことがあります。その場合でも、上記の command で有効化できます。

2. 次の prompt で QA agent に機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業します。
4. 完了したらレポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その成果をレビューしました。

- custom agent と instruction、skill、MCP tool の連携を確認しました。
- QA プロファイルを作成して確認しました。
- QA agent を選び、絞り込み issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらをまとめて機能 PR を作成し、[Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
