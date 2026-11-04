---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI tool と同様に、GitHub Copilot CLI にもさまざまな slash command があります。高度な機能、内部情報、追加の設定を利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利な command も確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキストを確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションを export または共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意の command を確認します。

## シナリオ

CLI の主要なワークフローが終わりました。ここでは、コンテキスト管理、モデル切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大 context window に達することがあります。Copilot CLI は必要に応じて会話を自動的に compact します。また、slash command で状態を確認したり、自分で compact したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は repository root から起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. model、現在の token 使用量、system instruction、tool、message、空き領域へのコンテキストの割り当てを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI は context window がいっぱいになると自動的に compact します。タイミングを自分で選びたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意な作業が異なり、開発者によって好みも異なります。Copilot CLI では、モデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI tool の活用を改善できます。`/share` は、セッションを Markdown または HTML ファイルに export したり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` の選択肢を確認します。
2. 共有する場合は、ローカルの Markdown export を作成する `/share file` など、目的に合った方法を選びます。
3. 誰かに送信したり公開したりする前に export 内容を確認します。session export にはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を並行化または委任する

主要なワークショップはここで完了です。Copilot CLI には、大きなタスク向けの command もあります。

- `/fleet` は独立した副タスクを subagent に割り当て、並行して実行します。
- `/worktree` は別の作業用に分離された Git worktree を作成します。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期で処理します。プルリクエストが作成される場合があります。

これらの command は追加の worktree やリモート作業を作成する可能性があるため、任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更し、セッションを共有し、内部で起きていることを確認できます。

- `/context` と `/compact` で会話コンテキストを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションを export・共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任を確認しました。

ほかにも多くの slash command があります。最後に、[学んだ内容を振り返り、次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI command reference][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
