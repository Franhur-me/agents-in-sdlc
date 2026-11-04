---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込みの実装、instruction の更新、skill の更新、品質保証（QA）profile、テストが、1 つの branch に保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト（PR）は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。このワークショップでは、手順を簡潔にするため、絞り込みと品質ワークフロー全体を 1 つの会話と branch にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化について学びます。
- 機能全体の変更と検証結果を確認します。
- 絞り込み PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能にするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビュー コメントに対応し、必要に応じて rebase するなど、PR の妨げになっている問題を解決します。その後、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せられますが、有効化する前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` command で新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功したらマージします。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、branch protection、merge queue、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリが許可する方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。

- Agent Merge と、マージ ライフサイクルの自動化について学びました。
- 機能全体の変更と検証結果を確認しました。
- 絞り込み PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ [Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI command reference][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
