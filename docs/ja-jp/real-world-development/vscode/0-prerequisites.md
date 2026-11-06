---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

コードを作成するリポジトリのコピーを用意するため、[template][template-repository] から自分用のリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. ラボ リポジトリのページで **Use this template** を選択します。続いて **Create a new repository** を選択します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターから案内された手順に従います。それ以外の場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後でラボで参照するため、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの issue を使うため、自分で起票する必要はありません。

## Codespace を作成する

次は、codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応するフル機能の IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりも早く完了します。その間に、次に扱う GitHub Copilot の機能を確認してもかまいません。

> [!CAUTION]
> 後の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で実行するように設計されています。どちらも、スムーズに進めるために必要な前提条件がそろった環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を使っている場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または Visual Studio Code で Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントで認証すると使用できる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウで「Hello world」のようなメッセージを入力し、Enter を押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。表示される手順に従って認証します。

    ![Copilot Chat の認証を求める画面の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書き始めるときに使用する codespace の作成を開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム命令][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
