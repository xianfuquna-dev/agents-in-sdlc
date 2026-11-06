---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot が依頼された内容だけでなく、どのように作業するかも理解できます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、Copilot がコーディング スタイルや要件を理解しやすくなるよう、コンテキストや設定を与えられます。好みのコーディング規約、ライブラリ、コードに含めるコメントの形式などを指定できるため、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の命令ファイルです。Copilot に送るチャットや CLI の多くのリクエストに関係する、プロジェクト レベルの情報を記述します。技術スタック、開発するものの概要、ベスト プラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う命令ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といったタスク向けのガイドラインを記述できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令と `applyTo` フロントマターが設定された `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の命令ファイルにも対応しています。[命令ファイルの対応状況][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルの情報を記述します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスクに関する命令を記述します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

命令ファイルの作成方法は 1 つではありません。AI の使い方と同様に、プロジェクトで試しながら適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルの作成を支援してもらうこともできます。各環境での方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。詳細が関係するレッスンで説明します。
>
> テンプレートや開始点を探していますか。[awesome-copilot][awesome-copilot] では、命令ファイル、カスタムエージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる命令ファイルを確認してください。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象を決めるリポジトリ ルートからの glob が設定されています。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトのユニットテストを作成するときに適用される命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令を小さく再利用しやすいファイルに分け、Copilot が参照できる例を示す方法です（ここにあるパスはリポジトリ ルートではなく命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc doc comment とファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、TSDoc doc comment とファイル レベルのコメント ヘッダーをコードに記述するというプロジェクト全体のルールです。後の演習で cloud agent に issue を割り当てると、このルールが適用されます。

編集する前に、作業用のブランチを作成します（cloud agent は issue の対象ブランチから命令を読み取りますが、変更をきれいにコミットしておきます）。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントについてのルールはありません。そのため、生成されたヘルパーに doc comment がありませんでした。
3. 次の Markdown を既存の標準のすぐ下に追加し、Copilot にファイル コメント ヘッダーと TSDoc doc comment を追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に置いて全体に適用することも、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` に置くこともできます。ここで追加した doc comment の規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記述します。

## 命令の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチにある命令ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。したがって、cloud agent が命令を利用できるように、変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てる issue を cloud agent が処理するときに、これらの命令を参照できます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` へコミットしてもかまいません。実際のプロジェクトと同じ手順を体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストにその標準が適用されることを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
