---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** は、普段使っているコード エディターに GitHub Copilot を統合します。Visual Studio Code と GitHub Codespaces で、エディターを離れずにエージェント モードの Copilot Chat、MCP による外部ツールへの接続、カスタムエージェントを使えます。Copilot はファイル、ターミナル、問題の全体を確認できます。

最初にカスタム命令を追加して Copilot が従うことを確認します。次にエージェント モードで UI、データ層、テストにわたるフィルタリング機能を作成します。その後、Playwright MCP サーバーを接続して Copilot にブラウザーから機能をテストしてもらい、プルリクエストを作成します。最後にアクセシビリティ用カスタムエージェントを確認して使い、エディターを離れずに Copilot の変更を監視、調整、反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | VS Code でカスタム命令を追加して確認する |
| [2. エージェント モード][ex2] | コード生成 | エージェント モードでフィルタリング機能を作成する |
| [3. Playwright MCP][ex3] | 外部ツール | Playwright MCP サーバーを使ってブラウザーで機能をテストする |
| [4. カスタムエージェント][ex4] | 特化エージェント | カスタムエージェントを確認して使用する |
| [5. エージェントの監視と管理][ex5] | 監視 | エージェント セッションを監視して方向付ける |
| [6. 反復][ex6] | レビュー | Copilot の作業をローカルで確認し、次の手順を選ぶ |
| [任意: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続する (3 モジュール) |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] GitHub Codespaces にアクセスできる

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。このワークショップで使う agent、MCP、code review、Copilot CLI の機能が **Copilot Student** プランに含まれています。

[callout-student-plan-education]: https://github.com/education/students

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
