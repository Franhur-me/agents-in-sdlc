---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、instruction file の編集や cloud agent が作成した成果のレビューに使う [codespace][codespaces] を起動します。

## 演習用リポジトリを設定する

作業用のリポジトリを作るには、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスには演習に必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選択し、**Create a new repository** を選んで自分のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後で参照するため、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

## Codespace を作成する

次に、演習で使う codespace を用意します。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりもずっと早く済みます。作成中に、次に説明する GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ここでは、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成し、Copilot cloud agent と作業するときに使う codespace の作成を開始しました。

## 次のステップ

cloud agent が従う custom instruction を追加しましょう。[演習 1 - Custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
