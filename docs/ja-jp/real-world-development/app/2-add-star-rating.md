---
title: "レッスン 2 - 星評価を追加する: すぐできる変更"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェント セッション**を開始して、プロジェクトに最初の変更を加えます。変更は小さくしましょう。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントに、プロジェクトへの小さく明確な変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのは、タイトル、カテゴリ、発行元、説明だけです。ウォームアップとして、エージェントに各カードで既存の評価を表示させます。これは小規模で独立した変更なので、最初のセッションにぴったりです。

## セッションの構成

**セッション**とは、専用の分離されたワークスペースで実行するエージェントとの会話です。各セッションには専用の Git worktree とブランチが用意されます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正しても、変更が競合せず、複数のセッションを同時に実行できます。セッションはサイドバーでリポジトリごとにまとめられています。セッションを選択すると、そのセッションに切り替わります。

セッションには、エージェントとの**会話**、ファイルを調査・編集する際のエージェントの**ツール アクティビティ**、差分を含む**変更ファイル**の一覧があります。

## セッションを開始して変更を依頼する

プロジェクトを確認して機能の実装を始めるため、新しいセッションを開始しましょう。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリに新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択します。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成して作業を始めます。次にプロジェクトを調査し、新機能を追加するために必要なファイルを特定してから、コードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューする必要があります。Copilot app で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルとその差分を確認します。

    ![Create PR の右にある Toggle review panel ボタンを指す矢印が表示された GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` に、コードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" と表示する次のような小さなコードです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは、決定論的ではなく確率的に動作するため、生成されるコードは上記と完全には一致しない場合があります。ただし、おおむね同じ内容になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動で実行したチェックの結果を確認します。テストで数値の `starRating` と `null` の場合の代替表示を確認していることを確かめてください。前提条件が不足している場合やチェックがスキップされた場合は合格ではありません。インストールの要求があれば、承認する前に内容を確認します。

もちろん、コードを読むだけで動作すると判断するべきではありません。Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新された UI を確認しましょう。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。この後のレッスンではカスタム キャンバスを調べたり、自分で作成したりします。ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトを使って、Copilot にアプリの起動とブラウザー キャンバスでのページ表示を依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使って、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを開いてマージする

機能を作成できました。次は、既存のコードベースに新しいコードをマージするプルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. サインインを求められた場合は、**Sign in with your browser** を選択し、表示される指示に従って認証します。
3. リポジトリ ポリシーによって PR 作成前に対応する Issue が必要だと Copilot が説明した場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー ペインで PR を開きます。ここで必要に応じて PR をレビューできます。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログ ウィンドウで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

おめでとうございます。GitHub Copilot app を使って最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を学びました。
- ゲーム カードに小さく明確な変更を加えるよう、エージェントに指示しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
