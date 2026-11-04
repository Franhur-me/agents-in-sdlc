---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みの絞り込み issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト（PR）を自分でマージしました。
- 絞り込み issue を起点に、Plan mode で進め方を決め、Autopilot mode で実装し、Interactive mode でレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、type check を実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーで絞り込み機能を確認しました。
- quality assurance (QA) custom agent を作成して選び、要件、テスト範囲、skill の結果、ブラウザーでの検証結果を評価しました。
- 絞り込みの変更全体をレビューし、絞り込み PR に Agent Merge を許可しました。
- context、model、共有、任意の cloud 委任に使う slash command を確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` からそれぞれ独立した branch を作り、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **絞り込みと品質ワークフロー:** 絞り込みを実装し、instruction を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA profile と関連テストを追加します。

絞り込みの計画から PR 作成まで、同じ会話と branch を使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認を実施しました。`quality-checks` skill は unit test、lint、type check を実行し、新しい形式で結果を報告します。QA agent は PR 作成前に、それらの結果と要件・テスト範囲のレビューをまとめました。

追加するテストは、実際の不足を補うものにします。QA を実行してもテストを追加する必要がない場合があります。マージを許可する前にコードと検証結果を確認し、変更後は関連する検証を再実行してください。

## ベスト プラクティス

Copilot に与えるコンテキストと tool が、その作業を左右します。このワークショップでは instruction を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を設定しました。会話をまたいでこれらの設定を再利用し、チームのニーズに合わせて調整します。instruction は標準を設定し、skill は繰り返し作業を説明し、custom agent は専門家の役割を定義し、MCP server は外部 tool と接続します。エージェントの要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合った **mode と model** を選びます。実装前に方針を考えるには **Plan**、範囲を絞った変更に参加しながら取り組むには **Interactive**、明確なタスクを任せるには **Autopilot** を使います。日常的な編集には高速な model、複雑な作業には高性能な model を選びます。

基盤と同じくらいコンテキストも重要です。何を作るのか、なぜ必要なのか、どう進めるのかを明確に伝えると、出力は大きく変わります。

## さらに試す

主要なワークフローを学びました。Copilot CLI には、ほかにも次の機能があります。

- `/review` で code review agent に変更を分析してもらう。
- `/rubber-duck` で問題を相談し、別の視点を得る。
- `/fleet` で独立した副タスクを並行して調整する。
- `/worktree` で別の作業を分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どの tool でも、上達する最善の方法は使い続けることです。本番コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学びましょう。いつものように、ドキュメントも確認してください。

Tailspin Toys をターミナルからさらに拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も体験する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI command reference][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
