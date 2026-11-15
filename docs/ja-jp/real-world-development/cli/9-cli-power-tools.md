---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、オプションのクラウドへの委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの優れた CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュコマンドが用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを操作できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、ワークツリー、cloud agent への委任に使えるオプションのコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模の大きいタスクや複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状況を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力して結果を比較します。会話が小さい場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関連のないタスクに切り替えるために新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使用できるモデルを一覧表示し、モデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## オプション: 作業の拡張や委任

ワークショップの主要な内容は完了です。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git ワークツリーを作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送信できます。cloud agent は非同期に作業し、プルリクエストを作成することがあります。

これらのコマンドは、追加のワークツリーやリモート作業を作成する可能性があるため、任意のものです。試すときは、まず範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の処理を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、ワークツリー、cloud agent への委任に使えるオプションのコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は[これまでの内容を振り返り][next-lesson]、今後の学習に役立つリソースを確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
