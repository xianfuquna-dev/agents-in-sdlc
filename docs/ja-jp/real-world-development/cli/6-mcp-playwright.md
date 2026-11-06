---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定して、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が使える自動化の追加が必要です。MCP サーバーを使うと、Copilot に CLI の組み込み機能以外のツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを確認します。
- 利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニットテストやエンドツーエンド テストは重要ですが、UI の更新を検証するには実際に操作する必要があります。Copilot が作業中の Web サイトを利用者と同じように操作できれば、変更が期待どおりに動作するか、より確実に自動確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を利用すると、最新情報へのアクセスやユーザーに代わった操作が可能になります。

ツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーごとに、エージェントが利用できるツールとリソースのセットが異なります。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促すために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同様に扱ってください。利用前にソース コードと発行元を確認し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーをまだ利用できない場合は、Copilot が利用者と同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、この手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を試してもらう

先ほどは機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーの使用を Copilot に伝えなくても自動的に判断されます。使うツールを指定すると、より一貫した結果が得られ、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- 未設定の場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
