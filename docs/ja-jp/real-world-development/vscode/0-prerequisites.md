---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な環境を準備します。Tailspin Toys repository のコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## 演習用 repository を設定する

作業用のコピーを作成するため、[template][template-repository] から repository instance を作ります。新しい instance には演習に必要な file がすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] を開きます。
2. 演習用 repository のページで **Use this template** ボタンを選択し、**Create a new repository** を選びます。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization に repository を作成します。

    ![repository template の設定を入力する](../../../_images/ex0-repository-settings.png)

4. 後で使うため、作成した repository の path（**organization-or-user-name/repository-name**）を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> template から repository を作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使うため、自分で作成する必要はありません。

## Codespace を作成する

次に、演習に使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの作成、実行、デバッグを行えるクラウド開発環境です。複数のプログラミング言語、extension、tool に対応する IDE を利用できます。

1. 作成した repository を開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要な service をすべて手動でインストールするよりも早く済みます。作成中に、次に紹介する GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件を用意します。ローカルで実行する場合は、クローンした repository を VS Code で開き、確認メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を使うには、IDE に GitHub Copilot Chat extension が必要です。GitHub Codespaces を利用している場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat extension がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。Visual Studio Code の Extensions view を開き、**GitHub Copilot Chat** を検索して **Install** を選択しても構いません。

extension のインストール後、GitHub account で認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、codespace 上部にある **Copilot Chat** icon を選択します。
3. Copilot Chat window に `Hello world` のような message を入力して <kbd>Enter</kbd> を押します。
4. 認証されていない場合は、GitHub account へのサインインを求められます。表示される手順に従って認証します。

    ![Copilot Chat の認証 prompt の例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat window が表示されます。

## まとめ

演習用 repository のコピーを作成し、コードを書き始めるときに使う codespace の作成を開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - Custom instruction][next-lesson]に進み、Copilot にプロジェクトの規約を教えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
