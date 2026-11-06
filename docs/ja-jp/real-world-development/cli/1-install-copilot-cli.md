---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、作成済みのフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コマンド ラインからコードベースを探索し、コードを生成し、コマンドを実行し、外部ツールを使えます。タスクを委任して変更を依頼できるため、作業の流れを保てます。まずはツールをインストールしましょう。使い慣れたツールで簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーでフィルタリング issue を見つけます。

## シナリオ

チームでは増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者がすでに使っているターミナルにその機能を提供します。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できるよう準備します。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たすことを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じて認証済みの場合があります。Copilot CLI の起動時に認証を求められなければ、そのまま使用できます。

## 使い方を確認する

通常のシェル プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問で Copilot が動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力して、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトで、フィルタリング issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の小さな変更を終えた後に、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチ上で作業し、自動的に worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は設定済みの権限の範囲外でツールを使用する前に確認を求めます。このワークショップでは、`--yolo` を付けて再起動します。ユーザーが承認したこのショートカットは Codespace 内の承認プロンプトを省略し、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は、すべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離環境でのみ使い、日常の開発用の既定コマンドとして alias に設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、この後のレッスンで issue やプルリクエストを操作するために使う読み書き可能な GitHub MCP ツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証された GitHub リソースには実際にアクセスできます。変更を公開またはマージする前に確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリ ルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話を確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりした後に、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルタリング issue を見つけました。

次は[最初の小さな変更を行い][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
