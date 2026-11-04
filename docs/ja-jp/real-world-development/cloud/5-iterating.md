---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

## 作業をレビューする

この演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが検討する関連ゲーム機能、高コントラストとライト モードを含むアクセシビリティ機能です。変更を確認し、必要に応じて Copilot にフィードバックを送り、改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チームにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期で、監視されていない状態でタスクを実行するため、安全を保つための制約があります。

- Copilot は repository に対して読み取り権限を持ち、書き込み権限は作業用 branch のみにあります。
- Cloud agent は GitHub Actions 上で、作業用の一時的な環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP server も対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成した PR を確認しましょう。コードへのドキュメント追加を、GitHub.com の標準 PR interface でレビューします。

> [!NOTE]
> PR に GitHub Copilot が firewall でブロックされたという警告が表示される場合があります。外部リソースや MCP server へのアクセスは既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent の firewall][agent-firewall]を設定または無効化できます。

1. github.com の repository に戻ります。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルの pull request を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc doc comment などの追加を確認します。実際の変更は異なる場合があります。Exercise 2 の instruction file による TSDoc、TypeScript 規約、コメント ヘッダーを確認してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちになっていることを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

9. PR の checks 領域に workflow が登録されたことを確認します。問題がなければ、単一 Astro app の project check が成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot と作業するのは一方通行ではありません。チーム メンバーに依頼するように、PR やコードへの inline comment で Copilot に `@copilot` を付けて依頼できます。Copilot はコメントを確認し、更新用の session を開始します。結果は予測できないため、必ず成功する prompt は提示できません。依頼できる変更の例を挙げます。

- 各コード file の先頭に役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro file に TSDoc doc comment を追加する。
- Astro app の構成を説明する README を作成する。

1. ドキュメントの変更を依頼し、他のユーザーと同じように **@copilot** を付けてコメントします。上記の例から選ぶか、別の依頼をします。
2. **View Session** を選択し、Copilot が更新する様子を確認します。Copilot は変更用の新しい session を開始します。
3. **Back to pull request** を選択して PR に戻れます。

   ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、新しい commit が PR に追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を繰り返せます。準備ができたら draft を review ready に変更し、`main` にマージします。

![PR を review ready に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. workflow が承認待ちになっていることを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

8. PR の checks 領域で workflow が実行され、単一 Astro app の project check が成功することを確認します。数分かかる場合があります。
9. **任意:** Codespace でこの branch に切り替えて関連ゲーム機能を手動テストできます。Codespace のターミナルで、Copilot が作成した branch 名（例: **copilot/fix-8**）に置き換えて次の command を実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を実装しました。Copilot cloud agent と反復して改善できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc doc comment の追加を依頼したりできます（この task は custom instruction を更新する前に割り当てました）。前と同じように **Conversation** タブでコメントすると、Copilot が新しい session を開始します。

## アクセシビリティ機能をレビューする

最後に、Accessibility agent を使って実装したアクセシビリティ機能をレビューします。この PR には、Exercise 3 で依頼した高コントラスト モードと、Exercise 4 で mission control から依頼したライト モードの両方が含まれます。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択してコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI component。
   - ユーザー設定を保存する local storage。
   - 高コントラスト モードとライト モードの CSS / style。
   - ARIA label やキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript / TypeScript。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちになっていることを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![workflow の実行を承認する](../../../_images/shared-approve-workflows.png)

9. PR の checks 領域で workflow が実行され、単一 Astro app の project check が成功することを確認します。数分かかる場合があります。
10. **任意:** Codespace で Copilot が作成した branch に切り替え、アクセシビリティ機能を手動テストできます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、高コントラストとライト モードを切り替えて、ページを再読み込みしても設定が保持されることを確認します。

Accessibility agent はアクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導きました。問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらに作業を委任する

Cloud agent に実際のバックログ作業を任せれば、その間に別の作業を進められます。習慣をつけるために、repository に issue を追加して Copilot に割り当ててみましょう。

- ゲーム詳細ページに backer interest form を作成する。
- game list page に pagination を実装する。
- data-access helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent の演習を完了しました。

- repository に含まれる custom instruction file を確認し、cloud agent の生成結果への影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境設定、計画、非同期実行を確認しました。
- custom agent を作成・使用し、高コントラストとライト モードの切り替えを追加しました。
- Copilot agents page を mission control として使い、進行中の session を監視・案内しました。
- cloud agent の PR をレビューして反復し、`@copilot` で変更を依頼し、workflow の実行を承認しました。

## レビューと次のステップ

Cloud agent の演習は完了です。ほかの環境も体験したい場合は、次の演習を確認してください。

- 🖥️ **[VS Code の演習](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP integration を使います。
- 💻 **[CLI の演習](../../cli/)** — Plan、Autopilot、agent skill、QA custom agent、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済み変更を 2 つリリースします。

自分の repository では、次の作業も試せます。

- refactoring や test coverage の issue を cloud agent に割り当てる。
- 別の分野の custom agent を作成する。
- 別の技術 stack 用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall の設定][agent-firewall]

---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
