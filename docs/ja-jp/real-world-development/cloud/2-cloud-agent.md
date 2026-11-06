---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩む organization は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能の要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間がレビューできるドラフトのプルリクエストを作成します。これにより、手間のかかるタスクを委任して開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする方法
- 操作を安全に実行する方法
- 範囲を明確に定めた issue の重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを適切に整備しませんでした。実際、ドキュメントがまったくないことに気付くでしょう。最初の改善として、アプリケーション内のすべてのエクスポートされた関数に TSDoc doc comment を追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示すれば、利用者は引き続きゲームを見つけられます。まずは UX の受け入れテストに使える簡単な実装を希望しています。これは現在のブロッカーですが、優先度の高い issue がほかにあります。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、非同期で処理している間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を記録するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、改善します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習ではカスタム命令と、Copilot を導く方法を確認しました。カスタム命令を手順に沿って設定した場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで少し確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認して、後でその影響を探せるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。`**/*.test.ts` ファイルにこれらの命令が適用されます。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てると、プルリクエストで TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認できます。後の演習でプルリクエストをレビューするときに、改めて取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、特定の環境と初期設定が必要です。タスクを割り当てた Copilot も同様で、ソフトウェア エンジニアのように作業を進めます。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業開始前に実行する特別なセットアップ ワークフローを `.github/workflows/copilot-setup-steps.yml` に設定すると、必要な開発ツールや依存関係を使用できるようになります。このラボの流れを円滑にし、学習に集中できるよう、あらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、移行とシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は自身の操作に専用トークンを使います。セットアップ手順に必要な最小限の権限を設定します。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中にリポジトリをクローンする場合は、contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースを移行、シードし、ビルドとテスト用のデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、次の点に注意してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトもあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、この作業は cloud agent に割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成している間、別の作業に取り組めます。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見つからない場合は、ページを更新してください。

16. 実行中のセッションをスクロールして、Copilot がどのように問題を解決するかを確認します。コードを調査して状態を理解し、適切な計画を考え、コードを作成していく過程を観察できます。

この作業には数分かかります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、開発者が別の作業に集中できるようにすることです。この特長を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能を追加する作業に進みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。デザイン チームが希望する関連ゲームの表示に時間がかからないとしても、その時間をほかの作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことを確認できます。

![issue の作業中であることを示す目の絵文字を Copilot が使用](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、継続的に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てて非同期で処理させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を学びました。

- コード生成環境のカスタマイズ
- 操作を安全に実行すること
- 範囲を明確に定めた issue の重要性
- issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
