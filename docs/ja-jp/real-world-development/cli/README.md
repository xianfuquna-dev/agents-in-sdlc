---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys の変更を 2 つ作成、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディング アシスタントとして利用できます。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンド ラインから作業できるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、使い方を確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリング issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの命令を更新して、フィルタリング作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加して、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の根拠を確認します。
8. フィルタリング全体の変更をレビューし、フィルタリング PR に Agent Merge を使います。
9. コンテキスト、モデル、共有、任意の cloud agent への委任に使う便利なスラッシュ コマンドを確認します。

ワークショップでは、星評価と、命令の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。フィルタリングと品質向上のワークフローは、同じ会話とブランチを共有するため、ツールを学びながら作業を積み重ねられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI をインストールする][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を確認し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue の機能を計画し、Autopilot で作成して Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認して更新し、フィルタリングに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最後の検証根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体を確認し、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認する |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ラインの基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。このワークショップで使う agent、MCP、code review、Copilot CLI の機能が **Copilot Student** プランに含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
