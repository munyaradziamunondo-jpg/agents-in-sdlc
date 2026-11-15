---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスごとの指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code やコマンドラインインターフェイス (CLI) の環境とは異なり、この環境では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub の課題を非同期に処理するため、リアルタイムで影響を並べて示すのが難しいためです。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な見た目にします。
- TypeScript のドキュメントコメント (TSDoc) をコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝えられます。これにより、より適切な提案やコードスニペットを得やすくなります。リポジトリ全体に適用する指示も、特定のファイル形式やタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、Copilot への多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの特定の言語や、UI コンポーネントまたはユニットテストの作成などのタスクに適用する指示ファイルです。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。詳しくは[カスタム指示のサポート][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な構成例が含まれています。基本的には、次の方針を参考にしてください。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ファイル形式 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに適用する指示を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの望ましい例および望ましくない例を示します。

指示ファイルの作成方法も AI の活用方法も、ひとつに決まっているわけではありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応する指示ファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。利用する環境によって方法は異なります (たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init`)。それぞれの環境に関するレッスンで、関連する場合に説明します。
>
> テンプレートやひな型を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある指示ファイルを読みます。共通の `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらの指示は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この値はリポジトリのルートからの glob で、指示を適用するファイルを決めます。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。大きな指示セットを小さな再利用可能なファイルに分け、Copilot がコード生成時に参照する例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーとして記載するという、プロジェクト全体のルールを加えることです。以降の演習で課題を割り当てると、cloud agent がこのルールを参照します。

編集の前に、作業用のブランチを作成します。cloud agent は課題の対象ブランチから指示を読み込みますが、変更をコミットするためにもブランチを用意します。

1. Codespace のターミナルで、次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コーディング標準を記載し、コード生成を改善します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加します。ファイルヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートされるすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを付けます。
   - import 文やコードより前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、特定の言語、ファイル形式、タスクに適用する指示は `*.instructions.md` に記載できます。TSDoc コメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は、課題が対象とするブランチから指示ファイルを読み込みます。次の演習で課題を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が更新した指示を使えるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で課題を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは、ブランチを作成せず `main` に直接変更をコミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを作成しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りの演習で cloud agent が作成するプルリクエストを確認すると、この標準が適用されていることが分かります。

次は、Copilot cloud agent に最初の課題を[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
