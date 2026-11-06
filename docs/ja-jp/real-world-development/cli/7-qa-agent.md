---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングを確認しました。次は、明確な QA プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリング issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化バージョンです。プロファイルでは、エージェントの目的、命令、利用可能なツールを指定します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは要件を評価し、結果を報告するために、これらの機能を Copilot がどのように使うかを示します。これらの代わりになるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと制約をプロファイルに反映したことを確認します。実装コードは変更せず、検証だけを行う場合でも、プロファイルが不完全だったり範囲が広すぎたりしないようにします。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が分かりやすくなります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードの変更前に確認を求め、変更をコミットせず、プルリクエストを作成しないことを確認します。
5. 責任や制約が不足している場合は、既定のエージェントに修正を依頼してから次に進みます。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開して QA を選択すると、コンテキストにある issue と計画の内容を使えます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドを実行すると有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリング issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートを確認する準備ができました。次は、それらをまとめて機能の PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
