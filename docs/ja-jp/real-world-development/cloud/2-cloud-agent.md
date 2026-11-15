---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな形で存在します。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。こうした手間のかかる作業を任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた課題の重要性。
- 課題を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した業務委託先は、ドキュメントを十分に作成していませんでした。まず、アプリケーションのエクスポート関数すべてに TypeScript のドキュメントコメント (TSDoc) を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧者が続けてゲームを探せるようにします。洗練された実装はまだ必要ありません。ユーザーエクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、さらに優先度の高い課題があります。

どちらも、優先度が下がりやすい一方で、Copilot cloud agent に任せるのに適したタスクです。cloud agent が非同期に作業する間、開発者は別のタスクに集中できます。作業が完了したら戻って結果をレビューし、期待どおりであることを確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。たとえば、[GitHub の課題を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエスト (PR) を作成し、環境を準備して、タスクに取りかかります。実行中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで開発者に通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、細部を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、適応させます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot の作業に与える影響を確認しました。カスタム指示の実習を終えている場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後から影響を判断できるようにします。

リポジトリの GitHub Web UI で、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を見ます。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` の課題を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。これらの指示ファイルが影響しているため、後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は特定の開発環境と、必要な状態に整えるセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。作業を始める前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を使用できるようになります。このラボの手順を円滑に進めるため、必要な設定は事前に用意されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを実行した SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順だけに限定した最小限の権限を設定します。Copilot は自分の操作用トークンを受け取ります。
    permissions:
      # たとえば、セットアップ手順でリポジトリをクローンして依存関係をインストールする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の終了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが存在しなかったりします。この種の技術的負債は対処されないままになり、開発効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成にも役立ちます。cloud agent に任せるのに適した課題です。バックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい課題ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `コードにドキュメントがありません` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、関数やメソッドの動作を明確にするために、TSDoc のドキュメントコメントを付けることを求めています。しかし、最近の更新ではこの標準が守られていません。動作を説明するうえで役立つ場所にドキュメントコメントを追加し、既存コードを更新する必要があります。
    ```

7. **Create** を選択して課題を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![課題に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは「[WIP]: Code lacks documentation」のようになります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかることがあります。Copilot は、事前に説明したワークフローを実行して環境を作り、プロジェクトを分析して、課題に取り組む方法を検討しています。

13. リストと、実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が課題の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて決定し、コードを作成する手順が表示されます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的は、タスクを非同期に実行し、開発者がほかの作業に集中できるようにすることです。この機能を使い、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点のひとつは、開発者がひとつの作業に取り組む間に、別の作業をエージェントに割り当てられることです。デザインチーム向けに関連ゲームの表示を追加する作業に時間がかからない場合でも、その時間をほかの作業に使えます。Copilot cloud agent に課題を割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい課題ダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示して、ゲームをさらに見つけやすくしたいと考えています。デザインチームは UX を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` に、指定したゲームと同じカテゴリのほかのゲームを返すデータアクセスヘルパーを追加します。指定したゲーム自体は除外します。
   - データアクセスヘルパーを使い、ゲーム詳細ページに「関連ゲーム」セクションを表示します。
   - 関連ゲームがない場合に対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

7. **Create** を選択して課題を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![課題に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、課題の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が課題の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。ここでは、カスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に課題を割り当てると、非同期に処理できます。技術的負債の解消、新しい機能の作成、フレームワーク間のコード移行などを Copilot に任せられます。

この演習では、次の内容を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲を明確に定めた課題の重要性。
- 課題を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業する間、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent では Model Context Protocol (MCP) サーバーも使えます][cloud-agent-mcp]。前の演習で確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub の課題を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
