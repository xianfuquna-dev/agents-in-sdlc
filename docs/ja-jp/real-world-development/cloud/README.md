---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

演習では、まず cloud agent が従うカスタム命令を追加し、GitHub issue を割り当てて実装を依頼します。カスタムエージェントを確認して利用しながら進め方を調整し、agents ダッシュボードからセッションを監視して方向付けます。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. GitHub Copilot cloud agent][ex2] | 非同期エージェント | Copilot cloud agent に issue を割り当てる |
| [3. カスタムエージェント][ex3] | 特化エージェント | カスタムエージェントを確認して使用する |
| [4. エージェントの監視と管理][ex4] | 監視 | エージェント セッションを監視して方向付ける |
| [5. 反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは organization で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。このワークショップで使う agent、MCP、code review、Copilot CLI の機能が **Copilot Student** プランに含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise では、管理者が organization に対して有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
