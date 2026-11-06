---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングを検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。手動でコードを検証し、命令ファイルを使って標準に従うようにしました。では、テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には**エージェント スキル**が適しています。スキルを使うと、こうした操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースがまとめられており、エージェントは必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、スキルの名前と説明を定義するフロントマター、役割の概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、呼び出し時にスキルが使うスクリプトやその他のリソースを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルをプロジェクトの `.github/skills` に配置すると、チームで共有して再利用できます。ユーザー用スキル フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明により、Copilot はスキルを呼び出すタイミングを判断します。
3. 命令を読み、テストと lint の手順をどのように案内しているかを確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI では、スキルを直接呼び出すことも、自然言語で依頼することもできます。3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したか、ツールが実際に何を報告したかが分かるレポートにします。スキルを更新して、この形式のレポートを作成しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを試します。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェック向けの `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリングの機能 PR に含めます。次は、[Playwright MCP サーバー][next-lesson]を通じて Copilot にサイトを直接操作させます。

## その他のスキルの例

これらのコミュニティの例は参考情報であり、追加の課題ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプトを含むスキル: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
