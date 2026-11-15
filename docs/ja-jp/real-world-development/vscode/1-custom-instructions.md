---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスごとの指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示に沿って、フィルタリング用の最初のデータスライス (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度送信し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリング用のデータスライスを `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部と異なる場合があります。Copilot の出力は確率的なものです。この演習の目的は、指示を更新した後の動作の傾向を確認することであり、出力を一字一句一致させることではありません。

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
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[ユーザーインターフェイス (UI) の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応する指示ファイルがあります。
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

## 変更用ブランチを作成する

ここでは、変更用のブランチを作成する一般的な方法を使います。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルの状態でプロンプトを送信します。その後、指示ファイルを更新して、同じプロンプトをもう一度送信します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的なため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コーディング標準を記載し、コード生成を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加します。ファイルヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートされるすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを付けます。
   - import 文やコードより前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、特定の言語、ファイル形式、タスクに適用する指示は `*.instructions.md` に記載できます。TSDoc コメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## プロンプトを再送信し、変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォーム用のパブリッシャーデータアクセスヘルパーです。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーを ID と名前とともに返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns パブリッシャーオブジェクトの配列に解決される Promise です。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。指示の更新をブランチにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の関連ファイルが表示されている場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエスト (PR) を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の課題と関連付けたプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。課題を見つけ、課題をクローズする PR を作成し、その後マージするように依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクトテンプレートでは GitHub ツールがワークスペースに接続済みのため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従って認証してください。後の演習では、この接続の仕組み (Model Context Protocol) を確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 次のプロンプトで、指示ファイルの更新に関する課題を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルタリング作業に使うパブリッシャーヘルパーの基盤の両方を説明に含めます。

   ```plaintext
   指示ファイルの更新に関する課題を探してください。現在のブランチから新しい PR を作成し、その課題をクローズする PR であることを記載してください。また、今後のフィルタリング作業に備えて、パブリッシャーヘルパーの基盤も追加したことを説明に含めてください。
   ```

4. Copilot が課題を探して PR を作成します。
5. Copilot に代わって GitHub 操作を実行することを求めるプロンプトが表示されたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードをプルして最新の状態にしてください。
   ```

7. Copilot に代わって GitHub 操作とシェルコマンドを実行することを求めるプロンプトが表示されたら、コマンドを確認し、適切な場合は **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージしました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示がある状態でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトをもう一度送信し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で作業を引き継げるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェントモード][next-lesson]を使い、Copilot にコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
