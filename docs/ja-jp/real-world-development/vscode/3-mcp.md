---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張について学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリングのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にアクセスできると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使って、エージェントが外部のツールやサービスと通信できるようにします。

![コンテキスト、LLM、ツール、および MCP サーバーや VS Code 拡張機能が提供するツールと連携するエージェント モードの仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を利用すると、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わった操作を実行したりできます。

ツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールとエージェントとの通信を管理します。MCP サーバーごとに、エージェントが利用できるツールとリソースのセットが異なります。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成や既存リポジトリの更新、issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。利用前にソース コードと発行元を慎重に確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っており、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot が操作できるブラウザーを利用できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは Playwright を画面のないブラウザーで実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright を使用しているため、必要なブラウザーはすでにインストールされていることが多いです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行してから再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを必要とするとき、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認して信頼し、起動を許可します。

手動での起動は任意です。省略した場合は、次の手順で Copilot が初めてツールを使うときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。利用者の代わりにエージェントがページを開き、移動し、フィルターを適用して、結果を確認して要約します。会話を中断せずに機能の動作を確かめる方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を使って判断します。そのため、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルタリング機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ基準に照らして内容を確認します。問題があれば、追加の質問をしたり、PR を作成する前にコードの修正を依頼したりします。

> [!NOTE]
> テストにはアプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が初めて Playwright MCP サーバーを使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行してから再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンド ライン インターフェイス (CLI) などで作成できます。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連 issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を Copilot に許可します。GitHub ツールを初めて使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot が issue を検索して適切なものを見つけ、PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだマージ**しないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot を拡張する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングの PR を作成しました。

MCP サーバーで機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティのレビューなどに使う[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
