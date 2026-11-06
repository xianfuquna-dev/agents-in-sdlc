---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ用リポジトリを設定する

コードを作成するリポジトリのコピーを用意するため、[template リポジトリ][tailspin-template]から自分用のリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. **Use this template** を選択し、次に **Create a new repository** を選択して自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターから案内された手順に従います。それ以外の場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成します。
4. 後でワークショップ内で参照するため、作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの issue を使って作業するため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使用してください。リポジトリ命令、アプリケーション コード、テスト、`quality-checks` スキル、バックログが含まれています。古いコピーを使用する場合は、必要なファイルがそろっているか、ファシリテーターに確認してください。

## Codespace を作成する

次は、codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応するフル機能のエディターを利用できます。

1. 新しく作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と尋ねられたら、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys サイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

これで準備完了です。このレッスンでは、テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成し、Codespace を作成してプロジェクトが使用できることを確認しました。

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
