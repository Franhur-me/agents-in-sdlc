---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリ custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここでは prompt の *更新前 / 更新後* を比較しません。Copilot cloud agent は GitHub issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのが難しいためです。この演習の後半で、cloud agent が作成するプルリクエストをレビューする際に instruction の効果を確認します。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

Instruction file を使い、これらのプラクティスに沿った作業を行うための情報を Copilot に渡します。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝えられます。使用するライブラリやコメントの種類なども指定できます。instruction はリポジトリ全体に適用するものも、特定のファイル種別に適用するものも作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用できます。TypeScript や Astro などの言語、UI component や unit test などの作業に関するガイダンスを提供します。

> [!NOTE]
> IDE では、instruction file は Copilot Chat によるコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの規格に沿った instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するベスト プラクティス

instruction file の作成に関する詳細はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用される標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに関するガイダンスに使います。
- 自然言語で明確に記述し、望ましいコードと望ましくないコードの例を示します。

instruction file や AI の使い方に唯一の正解はありません。プロジェクトに合った方法を試してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な instruction file を用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を生成してもらうこともできます。環境によって **Configure Chat → Generate Agent Instructions**（VS Code）や `/init`（Copilot CLI）など、操作が異なります。利用中の環境のレッスンで該当の機能を確認してください。
>
> ひな形や出発点には、instruction file、custom agent、その他のリソースをまとめた [awesome-copilot][awesome-copilot] も活用できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリにある中核の `copilot-instructions.md` と、タスク別の `*.instructions.md` をエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクト概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも、Copilot との作業全体に適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` frontmatter は適用対象の glob を定義します。ここでは `**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトで unit test を作成する際の instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。ほかの instruction file やプロジェクト内ファイルへのリンクがあり、ガイダンスを再利用しやすく分割し、参考例を示せます。（記載された path は repository root ではなく、この instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 新しいリポジトリ標準を追加する

この演習では、TSDoc doc comment とファイル レベルのコメント ヘッダーをコードに追加するルールを設定します。後の演習で issue を割り当てると、cloud agent はこの指示を読み込みます。

まず branch を作成します。cloud agent は issue の対象 branch から instruction を読み込みますが、変更を個別に commit しておきます。

1. Codespace のターミナルで新しい branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準にはコード内ドキュメントのルールがありません。
3. 既存の標準の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction は、リポジトリ全体に適用する `.github/copilot-instructions.md` にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加するようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## Instruction の変更を commit、push、マージする

Cloud agent は issue の対象 branch にある instruction を読み込みます。次の演習では `main` から branch を作って issue を割り当てるため、cloud agent が新しい instruction を使えるように、変更を `main` に入れます。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. branch を push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成してマージします。次の演習で issue に取り組むとき、cloud agent が新しい instruction を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合は branch の作成を省略して、`main` に commit します。ここでは実際のプロジェクトに近い手順を紹介しています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを得る方法を確認し、`.github/copilot-instructions.md` に新しい標準を追加しました。残りの演習で cloud agent が作成する PR に、その標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
