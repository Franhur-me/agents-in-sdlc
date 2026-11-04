---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、コンテキストを使えるようにします。Copilot に特定のコンテキストを渡す方法はいくつかあります。その中心となるのが [instruction file][instruction-files] です。コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- repository custom instruction とパス単位の instruction file を通じて、プロジェクトのコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- *現在の* instruction を使って、絞り込み機能の最初のデータ部分（publishers helper）を作成します。
- `.github/copilot-instructions.md` に repository 全体の標準を追加します。
- 同じ prompt を再実行し、生成コードが新しい標準に従う様子を確認します。
- instruction の更新と絞り込みの一部を `main` に commit し、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは設定した標準から外れることがあります。Copilot は確率的に動作します。この演習では文字単位で同じ出力を得るのではなく、instruction 更新後に傾向が変わることを確認します。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各 file の先頭に、file の役割を説明するコメントを追加します。

Instruction file を使い、これらのプラクティスに沿うための情報を Copilot に渡します。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案を得られます。使用する library やコメントの種類も指定できます。repository 全体に適用するものも、特定の file 種別に適用するものも作成できます。

- `.github/copilot-instructions.md` は repository への**すべて**の request で Copilot に送られます。技術 stack やプロジェクト概要など、プロジェクト全体に関係するコンテキストを記載します。
- `.github/instructions/*.instructions.md` は特定の file 種別やタスクに適用します。TypeScript や Astro、UI component や unit test などに関するガイダンスを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat によるコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時に repository レベルの instruction と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの規格に沿った instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するベスト プラクティス

この演習では詳しく扱いませんが、サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、プロジェクト概要、構成、コーディング標準などのプロジェクト全体のガイダンスを記載します。
- `*.instructions.md` は unit test、Astro component、データ レイヤーなどの file 種別や、特定のタスクに使います。
- 自然言語で明確に書き、望ましいコードと望ましくないコードの例を示します。

instruction file や AI の使い方に唯一の正解はありません。プロジェクトに合った方法を試してください。

> [!TIP]
> GitHub Copilot を使う project には十分な instruction file を用意しましょう。この project には [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けの file があります。
>
> Copilot に instruction file を生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境により操作が異なります。利用している環境のレッスンで該当の機能を確認してください。
>
> ひな形や出発点には、instruction file や custom agent などのリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

この repository にある `copilot-instructions.md` と、タスク別の `*.instructions.md` をエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクト概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも、Copilot との作業全体に適用されます。
3. `.github/instructions` folder を開き、Astro、Drizzle データ レイヤー、test などの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` field は repository root からの相対 glob を指定します。この例では `**/*.test.ts` に一致する TypeScript test file が対象です。
5. この project で unit test を作成するための instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。ほかの instruction file や project 内 file への link があり、ガイダンスを小さく再利用しやすく分け、Copilot が参照できる例を示しています。（path は repository root ではなく instruction file からの相対 path です。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。次の手順で TSDoc doc comment と file comment header のルールを追加します。

## 変更用 branch を作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押して新しい terminal を開きます。
3. 新しい branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction 更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model は演習ごとに異なるため、必要な手順で説明します。

custom instruction の効果を確認するため、まず現在の instruction のまま prompt を送信します。その後 instruction を更新し、同じ prompt を再送します。

1. 前の演習から残っている editor tab を閉じ、Copilot に必要な context だけを渡します。
2. Copilot が helper を作る場所を把握できるよう `src/lib/publishers.ts` を開きます。
3. Chat view の agent dropdown で **Agent** を選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot が project を調べて変更を適用します。helper file と test file の両方が変更されることがあります。
6. 提案された helper が型付き関数で、第一引数に `db` client を受け取り、publisher の型付き配列を返すことを確認します。これは `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約（`src/lib/*.ts` に適用）によるものです。
7. 生成されたコードに TSDoc doc comment と file-level comment header が**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくても doc comment を追加する場合があります。その場合も問題ありません。instruction 更新後に一貫性が高まることがこの演習の目的です。

## 新しい repository 標準を追加する

`.github/copilot-instructions.md` に標準を記述し、コード提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。生成された helper に doc comment がなかった理由は、コード内ドキュメントのルールがないためです。
3. 既存の標準の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> repository 全体の標準は `.github/copilot-instructions.md` に、特定の言語、file 種別、タスク向けの instruction は `*.instructions.md` に置きます。ここで追加した doc comment のような project 全体の標準には repository レベルの file が適しています。

## Prompt を再実行して変化を確認する

1. Copilot Chat で **New Chat** を選び、context をリセットします。
2. `src/lib/publishers.ts` を選択して、Copilot が対象 file に注目できるようにします。
3. 前と**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成された file の先頭に、次のようなコメント block があることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準を Copilot に適用し、次の演習に引き継ぐ実際のコードに反映しました。

## Instruction と変更内容を commit して push する

1. 新しい terminal を開くときは、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
2. 変更された file を確認します。

   ```bash
   git status
   ```

3. instruction と helper の変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch を repository に push します。

   ```bash
   git push -u origin custom-instructions
   ```

## Pull request を作成してマージする

branch を push したら、バックログのドキュメント標準に関する issue と関連付けて PR を作成します。GitHub tool が接続されているので、Copilot に issue を検索して PR を作成し、マージを依頼できます。

> [!NOTE]
> ここでは Copilot が初めて GitHub 上で操作します。template project には GitHub tool が設定済みなので、セットアップは不要です。最初の利用時に VS Code から GitHub へのサインインを求められたら、案内に従ってください。後の演習で、この接続の仕組み（Model Context Protocol）を確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat view を開き、agent picker で **Agent** を選択します。
3. instruction 更新と publishers helper の土台の両方を説明する PR を、現在の branch から作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub 操作の実行許可を求められたら、command を確認して許可します。
5. PR が作成されたら、PR をマージしてローカル branch を `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作や shell command の許可を求められた場合は内容を確認して、適切な操作を許可します。

GitHub Copilot の支援で PR を作成してマージできました。

## まとめと次のステップ

instruction file から Copilot がコンテキストを取得する仕組みを確認し、Copilot Chat を使って次の作業を行いました。

- *既存の* instruction を使ってコード生成 prompt を送り、結果を確認しました。
- `.github/copilot-instructions.md` に repository 全体の標準を追加しました。
- 同じ prompt を再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- instruction の更新と helper の土台を commit し、`main` に反映しました。

次は、[agent mode][next-lesson]を使い、Copilot にコードベース全体にわたる機能を追加してもらいます。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の personal custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
