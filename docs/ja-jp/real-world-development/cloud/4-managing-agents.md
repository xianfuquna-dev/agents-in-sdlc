---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent にユーザー エクスペリエンスの改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば GitHub.com の[agents ページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、実行中のセッションに追加の指示を与えたりすることもできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼し、作業の方向を調整します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能を作成しているため、一緒に実装するのが合理的です。エージェントの作業を方向付け、ハイ コントラスト モードとライト モードの両方を追加しましょう。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てた次のタスクを確認します。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイ コントラスト モードの追加
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、次のような作業があります。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーを設定する
   - 割り当てられたタスクを完了するための各種操作を行う

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## Cloud agent に追加の指示を与える

実行中のタスクを確認できたので、ハイ コントラスト モードを作成中の Copilot にライト モードも追加するよう依頼します。

1. ハイ コントラスト モードのタスクを参照するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、作業中のセッションに指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

前と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼が作業に取り込まれます。

この作業にも数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent の監視と案内に使う中心的な場所である Copilot agents ページを確認しました。mission control を使って、進行中と完了済みのタスクを追跡し、実行内容をレビューし、実行中のタスクの範囲を変更したり追加の指示を与えたりできます。

次の概念を学びました。

- mission control と agents ページで cloud agent のタスクを監視する
- 実行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復的に改善する][next-lesson]方法に進みます。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
