---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数ファイルの変更やテスト実行などの作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更を行い、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけて再度更新します。作業は次々に増えていきます。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE 内でより自律的に作業します。既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに作られています。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームのペア プログラマーとして非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを見つけ、複数の変更を調整する方法
- UI とデータ層にわたって機能を実装する方法
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えているため、利用者がカテゴリとパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot エージェント モードで行います。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームで、利用者はゲームの一覧と詳細を確認できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う範囲の小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後であれば、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

この Web サイトはすでに動作しますが、新しい機能を追加します。プロジェクトをテンプレートから作成したときに、issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。エージェント モードは、ファイルにわたる変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。このワークショップでは Claude Sonnet 4.5 を使用しており、手順の再現性を高めるために記載しています。ただし LLM は確率的なため、提案は異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な特性により、Copilot が別の MCP コマンドを使うことがありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者に最も役立つと考える項目を優先順位付けします。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える命令ファイルを確認します。Web サイトへの機能追加方法に関するコンテキストを含む [ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関するガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト可能性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle などのファイル別命令へのリンク

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所からの相対パスです。命令を再利用しやすく分割し、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つを変更する必要があります。

- データ層 (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する
- フィルタリングの動作に関するテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

コードベースにマージする前に、テストを実行して成功を確認する必要もあります。Copilot エージェント モードにこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例を示す画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 関連するバックログ issue の要件に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、対象のファイルを見つけて、データ層ヘルパー、UI、テストを変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクとコードベースの複雑さによって数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業の途中で選択する必要はなく、完了するとすべての変更に対して選択できます。

7. Copilot に求められたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat ペインでテスト実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクを終えてエラーを検出しなくなるまで作業します。

   ![Copilot エージェント モードの会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使って Tailspin Toys に機能を追加する方法を確認しました。次のことを学びました。

- UI とデータ層にわたる新機能を実装する方法
- プロジェクトを調べ、関連ファイルを見つけ、変更を調整する方法
- コードベースにマージする前に、変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページングを実装する

ゲーム一覧が増えたら、ページングが必要です。この演習で学んだ方法を使って、Copilot にページングを追加するよう依頼します。次の点を検討してください。

- 既存の命令ファイルを使うなど、ベスト プラクティスに従います。
- ページ サイズを利用者が選べるようにするか、固定にするか決めます。
- 希望どおりの実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更やコンテキストを伝え、反復的に改善する必要がある場合があります。Copilot と作業する通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
