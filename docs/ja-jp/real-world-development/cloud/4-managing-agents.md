---
title: "演習 4 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、進捗を監視することも重要です。

GitHub の [agents page][agents-page]など、cloud agent に割り当てたタスクを管理する tool がいくつかあります。この mission control では、open pull request (PR) がある agent task を確認し、作業中の session に追加の指示を出せます。

このレッスンでは、次のことを行います。

- agents page で cloud agent のタスクを監視します。
- 実行中の session に追加の機能を依頼します。

## シナリオ

高コントラスト モードの実装を依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているところなので、高コントラストに加えてライト モードも実装するよう agent に伝えます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents page](https://github.com/copilot/agents) を開きます。
2. メイン領域と左側の一覧にあるタスクを確認します。次のようなタスクが表示されます。
   - codebase のドキュメントを更新する。
   - game を変更する API を作成する。
   - Web サイトに高コントラスト モードを追加する。
3. 実行中の task を 1 つ選択し、Copilot が実行した操作を確認します。
   - repository の checkout。
   - 作業環境の作成。
   - MCP server の設定。
   - タスク完了に向けた各種操作。

> [!NOTE]
> 表示される手順は、Copilot の進捗や作業方法によって異なります。

4. 右側に表示される PR pane で、PR と変更された file を確認します。

## Cloud agent に追加の指示を出す

実行中のタスクを確認できたので、高コントラスト モードを作業中の Copilot にライト モードも追加するよう依頼します。

1. 高コントラスト モードの session を選択します。正確なタイトルは Copilot が付けた名前や進捗によって異なります。

   ![Mission control に表示された accessibility session](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり作業が始まるまで、session を数分確認します。
3. **Steer active session while Copilot is working** dialog に、次の prompt を入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents page の cloud agent task で、実行中の session を案内する dialog が選択されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業に含めることを確認します。

## Copilot の作業を待つ

更新した依頼に対して Copilot が作業します。メッセージを送信した時点で実行中だった手順を終えた後、新しい要望を作業に含めます。

処理には数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents page は、GitHub Copilot cloud agent のタスクを監視して案内するための中心的な場所です。実行中・完了済みタスクと進捗を確認し、実行中のタスクに追加の指示を出せます。

- agents page から cloud agent のタスクを監視しました。
- 実行中の session に追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成した PR を反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
