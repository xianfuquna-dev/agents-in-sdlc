---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。コード生成のガイダンスを伝える主な方法の 1 つが[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の命令を使って、フィルタリング用データ スライスの最初の部分（パブリッシャー ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 命令の更新とフィルタリングの作業を `main` にコミットし、次の演習で Copilot が更新したガイダンスを利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部と異なる場合があります。Copilot は確率的に動作します。この演習では、命令を更新した後の動作の**傾向**を確認します。出力が文字単位ですべて一致することを期待するものではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot が理解しやすくなるよう、コンテキストや設定を与えられます。好みのコーディング規約やライブラリ、コードに含めるコメントの形式などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定のファイルの種類に適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストで Copilot に送信される単一の命令ファイルです。技術スタック、開発中のものの概要、ベスト プラクティス、全体に適用するガイダンスなど、プロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う命令ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といったタスク向けのガイドラインを記述できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令と、`applyTo` フロントマターが設定された `*.instructions.md` ファイルを両方使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の命令ファイルにも対応しています。[命令ファイルの対応状況][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスク向けの命令を記述します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

命令ファイルの作成方法は 1 つではありません。AI の使い方と同じように、プロジェクトで試しながら適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルの作成を支援してもらうこともできます。環境によって方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。詳細が必要な場合は、該当する環境のレッスンで説明します。
>
> テンプレートや開始点を探していますか。[awesome-copilot][awesome-copilot] では、命令ファイル、カスタムエージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらの命令は、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象を決めるリポジトリ ルートからの glob が設定されています。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトのユニットテストを作成するときに適用される命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある `unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。命令を小さく再利用しやすいファイルに分け、Copilot が参照できる例を示せます（ここにあるパスはリポジトリ ルートではなく命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後、命令を更新して同じプロンプトを再送信し、違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを作成する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、コードの変更を適用します。ヘルパー ファイルとテストの両方が変更されることがあります。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 生成されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。命令を更新した後に動作の一貫性が高まることがこの演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存のコーディング標準には、生成されたヘルパーに doc comment がなかった理由となる、コード内ドキュメントの規則がありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に置いて全体に適用することも、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` に置くこともできます。ここで追加した doc comment の規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記述します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択して、Copilot に対象のファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォームのパブリッシャー データ アクセス ヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns パブリッシャー オブジェクトの配列に解決する Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で拡張する実際のコードに適用しました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもリポジトリ内のほかのファイルと同じようにソース管理できます。命令ファイルとブランチをコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の更新とヘルパーの基盤に関する、Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているため、Copilot に issue を探して PR を作成し、マージを依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインが必要になる場合があります。画面の指示に従って認証してください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 命令ファイルの更新とパブリッシャー ヘルパーの基盤の両方を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot による GitHub 操作の実行を求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージと `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot による GitHub 操作とシェル コマンドの実行を求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の命令を使ったコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用されることを確認しました。
- 次の演習で使えるよう、命令とヘルパーの基盤を `main` にコミットしました。

次は[エージェント モード][next-lesson]で命令を使い、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
