---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan で進め方を決め、Autopilot で issue に基づく絞り込みを実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しましたが、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は既存の組織の開発フローに沿って動作するよう設計されており、適切なものを適切な方法で作成できます。このレッスンから、issue を起点に機能を作り、コードの妥当性と機能の動作を確認し、最終的にプロジェクトへマージする、一般的なエージェント駆動の開発プロセスを学びます。

> [!NOTE]
> この機能の作業中は、同じ会話と branch を使います。通常はファイル種別ごとに branch や PR を分けますが、ここでは主要な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を開始します。
- Plan mode で要件を定義します。
- Autopilot mode で機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能開発を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要です。Plan mode で決定事項を整理し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 必要な作業を記述した issue から始める。
2. 実装計画を立てる。
3. コードを作成してレビューする。
4. テストを実行して検証する。
5. 新機能を手動で検証する。
6. プルリクエスト（PR）を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、ほとんどは上記の流れに近いものです。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次の操作に進む前にあなたの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan mode で計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み作業に取り掛かる前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新して、絞り込み用の branch を作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト（PR）をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、機能の実装やタスクを Copilot に任せる前に、進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作ります。

3. 絞り込みの動作について質問されたら、希望する仕様を回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して計画を展開します。
5. 計画を確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、Autopilot で実装します。Autopilot は、完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足していたり port が競合したりする場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. 結果を issue と計画時の決定事項に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードが良さそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces の転送 port から開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces に port `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、発行元、および組み合わせの絞り込みを試します。
4. リセット時と結果が空の場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発 server を停止するよう依頼します。

## まとめと次のステップ

複数の会話モードを使って、機能を実装しレビューしました。

- GitHub issue から Copilot 会話を開始しました。
- Plan mode で要件を定義しました。
- Autopilot mode で機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[custom instruction を使ってコード生成をさらに詳しく確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI command reference][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
