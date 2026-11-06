---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーに Markdown ファイルを置き、集中した指示、ベスト プラクティス、コーディング パターン、分野固有の知識を定義すると、Copilot がその種類の作業を効果的に実行できます。チームの専門知識を再利用できるエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを実践するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。分かりやすく区別するなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキルやほかのエージェントを調整する役割を担います。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー スキルやテストを実行、管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを学びます。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近の利用者からのフィードバックによると、テキストと背景色のコントラストが不十分なため、ダーク テーマでは読みにくいと感じる人がいます。このアクセシビリティ上の課題に対処するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。カスタムエージェントを使って機能を作成しましょう。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのように導くか、内容を確認しましょう。

codespace に戻り、アクセシビリティ カスタムエージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と間違い

## issue を作成して割り当てる

Mission control は、この環境のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を与えられます。まず、ハイ コントラスト モードの作成を Copilot に依頼しましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントと「Accessibility」が強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot の作業を導けます。

次の概念を学びました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して方向付ける][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [organization でカスタムエージェントを使うための準備][org-custom-agents]
- [enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
