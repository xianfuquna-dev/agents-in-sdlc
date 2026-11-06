---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、命令、スキル、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常は機能、命令、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質に関する一連の作業を 1 つの会話とブランチで行い、1 つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを許可する権限を維持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合のリベースなど）に対処し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りの手順を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されている場合は、レビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成やテスト、検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
