---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。コードを編集する手順ファイルや、cloud agent が作成した内容のレビューに利用します。

## ラボ用リポジトリを設定する

コードを作成するリポジトリのコピーを用意するため、[template][template-repository] から自分用のリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、自分用のコピーを作成します。続いて **Create a new repository** を選択します。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターから案内された手順に従ってください。それ以外の場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後でラボで参照するため、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

## Codespace を作成する

次に、codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応するフル機能の IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも早く完了します。その間に、次に扱う GitHub Copilot の機能を確認してもかまいません。

> [!CAUTION]
> 後の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で実行するように設計されています。どちらも、スムーズに進めるために必要な前提条件がそろった環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業するときに使う codespace の作成を開始しました。

## 次のステップ

cloud agent が従うカスタム命令を追加しましょう。[演習 1 - カスタム命令][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
