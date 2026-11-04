---
title: "レッスン 4 - Custom instruction で Copilot を導く"
description: "リポジトリ instruction を確認し、ドキュメント標準を追加して、絞り込みコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。絞り込み機能を実装したので、Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ instruction とパス単位の instruction file がエージェントに届く仕組みを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントでは、コードの説明を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro component は `Props` 契約を文書化し、関連コードの変更時にコメントを更新します。
- 既存の format と lint のガイダンスを維持します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## Instruction file

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案を得られます。コーディング規約、使用する library、含めたいコメントの種類なども指定できます。リポジトリ全体に適用するものも、特定のファイル種別に適用するものも作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に渡される単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイダンスを提供します。

> [!NOTE]
> その他の instruction 形式の対応状況は環境によって異なります。形式に頼る前に、[custom instruction の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには instruction file が含まれています。変更を加える前に、内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明と、このリポジトリ全体に適用されるガイダンスを確認します。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象の glob を定義しています。
6. `.github/instructions/drizzle.instructions.md` を開き、関連 instruction file やプロジェクト内ファイルへのリンクを確認します。instruction を小さな再利用可能な単位に分け、参照例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存の instruction に加えて、生成する TypeScript に TSDoc コメントを追加する標準を設定します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション（35 行目付近）を探します。
2. 最後の箇条書きに次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリ instruction を読み込みます。更新したガイダンスを読み込ませるため、フィルタリング会話を終了して再開します。

1. フィルタリング会話が開いている場合は `/exit` を入力します。Copilot CLI が会話を自動的に保存します。
2. リポジトリ root の `game-filters-cli` branch で、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルタリング会話を選び、`game-filters-cli` branch が checkout されていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。TSDoc コメントが追加され、正確に説明されていることを確認します。

## まとめと次のステップ

Copilot が instruction file を使う仕組みを確認し、機能に新しい標準を適用しました。

- リポジトリ instruction とパス単位の instruction file の役割を確認しました。
- instruction file を更新し、コーディング標準を設定しました。
- instruction file がコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [Custom instruction の対応状況][custom-instructions-support]
- [効果的な custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
