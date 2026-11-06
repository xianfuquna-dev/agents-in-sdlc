---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加について GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザイン チームが改善できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要なら Copilot にフィードバックを伝えて改善しましょう。

### シナリオ

ここまでに何度も確認したとおり、生成 AI を利用してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしに非同期でタスクを実行するため、安全を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのはコードを作成するためのブランチに限られます。
- Cloud agent は GitHub Actions 内で実行され、作業用の一時的な独立環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメントの追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、既定で外部リソースへのアクセスを制限されているため、これは想定どおりの動作です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment やその他のドキュメントが追加されたコードを確認します。変更内容は状況によって異なります。演習 2 で確認したカスタム命令に基づく TSDoc doc comment、TypeScript の規約、コメント ヘッダーを探します。
6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方向のやり取りではありません。チームのほかのメンバーと同じように、プルリクエストやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための別のセッションを開始します。結果は確定的ではないため、必ず使うべきプロンプトはありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. これらのアイデアや、コードに追加してほしいドキュメントについての別の提案を使い、ユーザーと同じように **@copilot** をメンションしてコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、必要に応じて作業を続けます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![レビュー可能な状態に PR を変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して反復的に改善できます。たとえば表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc doc comment を追加したりできます（これはカスタム命令を更新する前に割り当てたタスクです）。ほかの PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注意します。
   - 表示モードを切り替える UI コンポーネント
   - 利用者の設定を保持するための local storage の使用
   - ハイ コントラスト モードとライト モードの CSS やスタイル変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript / TypeScript のコード

6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベスト プラクティスに沿ってこれらの機能を実装するよう Copilot を導いたことを確認してください。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

Cloud agent は、バックログにある実際のタスクを任せ、その間に別の作業を進めると効果的です。いくつか issue を作成して Copilot に割り当て、この方法を練習してください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データ アクセス ヘルパーに入力検証とエラー処理を追加する

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- このリポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力に及ぼす影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使用し、ハイ コントラストとライト モードを追加しました。
- Copilot agents ページを mission control として使い、実行中のアクセシビリティ セッションを監視して方向付けました。
- Copilot のプルリクエストをレビューして改善し、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学びたい場合は、ほかのハーネスで今回の内容をさらに実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリで、次のアイデアも試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる
- 別の分野向けのカスタムエージェントを作成する
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
