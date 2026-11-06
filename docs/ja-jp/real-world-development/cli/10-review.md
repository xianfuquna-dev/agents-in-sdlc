---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を Tailspin Toys の一連のワークフローで使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、作成済みのフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証を確認しました。
- フィルタリング全体の変更をレビューし、フィルタリング PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済みの `main` から作成する 2 つの PR があります。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルタリングと品質のワークフロー:** フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

ワークショップを簡潔にするため、フィルタリングの計画から PR 作成まで、同じ会話とブランチを使用して 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは、ユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、その結果と要件、テスト カバレッジのレビューをまとめて PR の前に確認しました。

追加するテストは実際の不足を補うものである必要があります。新しいテストが不要と判断される QA 実行も適切な場合があります。マージを許可する前に、コードと根拠を確認し、変更後は関連する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話を越えてこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。命令は標準を設定し、スキルは繰り返し実行するタスクをまとめ、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に進め方を検討するとき、**Interactive** は範囲を絞った変更に参加しながら取り組むとき、**Autopilot** は範囲が明確なタスクに適しています。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るか、なぜ作るか、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主なワークフローを学びました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点からの提案を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。製品コード、趣味のコード、何年も作りたかった小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
