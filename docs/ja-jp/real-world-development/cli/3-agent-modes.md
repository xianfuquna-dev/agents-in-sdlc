---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue に基づくフィルタリングを実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の流れに合わせて作業できるため、適切な方法で必要な機能を作成できます。このレッスンから数回にわたり、issue から新機能を作り、コードの妥当性と機能の動作を確認し、プロジェクトに正常にマージするという、一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能を完成させるまで、同じ会話とブランチを使います。実際には、ファイルの種類ごとにブランチや PR を分けることが多いですが、ここでは主な概念に集中するために簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot との会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続ける中で、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、利用者がカテゴリとパブリッシャーでゲームを絞り込めるようにします。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードでこれらを決めた後、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発に取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードがレビューされ、継続的インテグレーションが成功したらマージします。

> [!NOTE]
> 具体的な手順はチームや organization によって異なりますが、多くは上記の流れに近いものです。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントがどれだけ自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成し、プロジェクト ファイルを編集できない状態になります。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能を実装したりタスクを Copilot に渡したりする前に、進め方を定義できます。複雑な作業では、少し時間をかけて計画を立てると効果的です。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に合わせて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画をレビューします。
6. 希望と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が問題を解決したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. issue と計画時に決めた内容に照らして結果を確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリ フィルター、パブリッシャー フィルター、それらの組み合わせを試します。
4. リセットと結果が空になる場合の動作が、issue と決定した内容に合うことを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム命令を使って][next-lesson]コードが文書化された規約に従うようにし、コード生成についてさらに学びます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
