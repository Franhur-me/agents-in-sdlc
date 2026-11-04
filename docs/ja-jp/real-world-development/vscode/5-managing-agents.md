---
title: "演習 5 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、codebase を調査し、変更を提案し、file を編集し、command を実行します。VS Code の Copilot Chat は、作業中の tool call と file 編集を表示します。diff を確認し、変更を個別に採用または取り消し、editor を離れずに follow-up prompt で作業を調整・拡張できます。

この演習では、次のことを行います。

- accessibility agent の作業をリアルタイムで監視します。
- 提案された file 編集をレビューして採用します。
- session の途中で agent に指示し、高コントラストに加えてライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で使った accessibility custom agent は、高コントラスト モードを実装しています。作業中にデザイン チームから、既定のテーマが暗すぎると感じる利用者向けにライト モードも追加してほしいという依頼が来ました。新しい会話を始める代わりに、既存の session を案内して作業を拡張します。

## Copilot の進捗を監視する

Copilot Chat view には、agent が調べた file、実行した command、提案した編集が順に表示されます。file 編集は diff と **Keep** / **Undo** の操作とともに chat に表示されます。

1. 演習 4 から Copilot Chat view に戻ります。
2. agent が作業している間、chat をスクロールし、tool call の結果を確認します。file の読み込み、編集、terminal command などが表示されます。
3. agent が file 編集を提案したら、diff を確認します。
   - **Keep** を選択すると変更を採用します。
   - **Undo** を選択すると変更を取り消し、agent に再試行させます。
4. Chat の上部にある **Files changed** indicator で、この session 中に変更された file 全体を確認できます。

> [!NOTE]
> terminal command の実行などに許可が必要な場合、agent は処理を一時停止し **Allow** を表示します。内容を確認し、許可してよい command のみ承認します。

5. agent が高コントラスト モードの実装を終えるまで待ちます。最後に変更内容の要約が表示されます。

## Agent にライト モードを追加するよう依頼する

高コントラスト モードができたら、同じ会話でライト モードの切り替えを追加します。会話を続けることで、agent は前の作業、変更した component、style、local storage の使い方を保持します。

1. Copilot Chat の入力欄に次の follow-up prompt を送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent が前の変更を確認し、ライト モードを追加する様子を確認します。
3. 新しい編集を確認し、各 diff で **Keep** または **Undo** を選択します。
4. agent の進め方が希望と違う場合（例: toggle UI を意図せず refactor している場合）、Chat 入力欄の隣にある **Stop** button で停止し、追加の説明を送ります。

> [!TIP]
> 実行中の session に指示を追加すると、最初からやり直すより早く修正できます。follow-up prompt で「CSS を単純にし、selector を重複させず CSS variable を使う」「3 つ目の toggle を追加せず、既存 settings panel にライト モードを組み込む」「両方の toggle に keyboard shortcut を追加する」などと依頼できます。

## 作業内容をレビューする

commit 前に agent が変更した内容を確認します。

1. VS Code の **Source Control** view を開きます。
2. 変更された file の一覧を確認します。Astro component、style、関連 test などが含まれます。
3. いくつかの file を開いて diff を確認します。ARIA 属性、keyboard navigation、semantic HTML、local storage による永続化など、custom agent が指示されたアクセシビリティ パターンに従っていることを確認します。
4. 問題がなければ Source Control panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode で実行中の session を監視して案内し、各 file の編集を確認して採用または取り消し、context を保ったまま follow-up prompt で作業を拡張しました。

- agent の作業をリアルタイムで監視しました。
- 個々の file 編集を採用または取り消しました。
- session の途中で追加の指示を送りました。

次の[レッスン][next-lesson]では、アクセシビリティ機能を PR として公開し、ローカル custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
