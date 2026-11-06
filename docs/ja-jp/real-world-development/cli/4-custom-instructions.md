---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、Copilot がそのガイダンスを参照できるようにします。[命令ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルタリングを実装したので、Copilot が使用した命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパスに応じた命令ファイルがエージェントにどう伝わるかを確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの繰り返しではなく、目的や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。注入可能な `db` 引数がある場合は、それも含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたらコメントも最新の状態にします。
- 既存の書式と lint に関するガイダンスを維持します。

命令ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件を Copilot が理解しやすくなるように、コンテキストや設定を与えられます。好みのコーディング規約やライブラリ、コードに含めるコメントの形式などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定のファイルの種類に適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に送信される単一の命令ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスク向けのガイドラインを記述するファイルです。

> [!NOTE]
> ほかの形式の命令と対応状況は、環境によって異なります。特定の形式を利用する前に、[カスタム命令の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、作業を始めるための命令ファイルが含まれています。変更を加える前に内容を確認して、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。これらの命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令の適用対象を決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクト ファイルへのリンクを確認します。命令を小さく再利用しやすいファイルに分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。新しく生成される TypeScript に TSDoc を追加するため、基本の `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` を開き、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。更新した命令を適用するには、機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの命令とパスに応じた命令ファイルがエージェントにどう伝わるかを確認しました。
- コーディング標準を守るため命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令の対応状況][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
