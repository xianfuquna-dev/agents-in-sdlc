---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤にしたデスクトップ アプリケーションです。エージェント主導の開発に集中して取り組める単一のワークスペースを提供し、エージェント セッションの並行実行、セッション モードの切り替え、共有 Canvas、GitHub issue とプルリクエストのネイティブな管理などに対応しています。さらに **Agent Merge** を使うと、プルリクエストのリベース、レビュー フィードバックへの対応、継続的インテグレーション (CI) の修正、マージを進められます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. プロジェクトを準備し、アプリをインストールし、リポジトリに接続して、ワークスペースと作成済みのバックログを確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリング issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの命令を更新して、フィルタリング作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加して、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の根拠を確認します。
8. フィルタリング全体の変更をレビューし、2 つ目の PR に Agent Merge を使います。
9. Canvas を利用できるか確認し、Database Explorer を使ってリポジトリに紐づくトリアージ Canvas を作成してテストします。必要な機能を使えない場合は、その障害を記録して読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップでは、星評価と、命令の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。どちらも更新済みの `main` から開始します。フィルタリングと品質向上の作業は、同じセッション、worktree、ブランチを共有するため、ツールを学びながら作業を積み重ねられます。最後の Canvas 演習も同じセッションに残し、PR ワークフローを繰り返さずに共有ワークスペースの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys のコピーを作成する |
| [1. Copilot app をインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトに接続し、ワークスペースを確認する |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を確認し、PR 1 をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue の機能を計画し、Autopilot で作成して Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認して更新し、フィルタリングに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家プロファイルを作成して選択し、最後の検証根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、命令、スキル、QA プロファイル、テストを確認し、2 つ目の PR に Agent Merge を使う |
| [9. Canvas を確認して作成する][ex9] | 共同作業 | Database Explorer を使い、リポジトリに紐づくトリアージ Canvas を作成してテストする |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加リソースを確認する |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに [Git][install-git] をインストールしている

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。このワークショップで使う agent、MCP、code review、Copilot CLI の機能が **Copilot Student** プランに含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。そのため、[前提条件の演習][ex0]で Copilot app をインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。これは Copilot CLI のポリシーとは別の設定です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
