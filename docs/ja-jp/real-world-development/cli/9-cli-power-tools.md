---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同じように、GitHub Copilot CLI にもさまざまなスラッシュコマンドがあります。高度な機能、内部の情報、追加の設定オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。このレッスンでは、さらに便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って Copilot のコンテキスト管理を確認します。
- `/model` を使って利用できるモデルを調べます。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを完了しました。次は、コンテキスト管理、モデルの切り替え、セッション共有など、追加機能を確認します。必要であれば、[Copilot cloud agent][about-cloud-agent]に作業を委任できます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウを超える場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使うと、コンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI がまだ起動していない場合は、リポジトリルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキストの配分を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が小さい場合は大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的に会話を圧縮します。`/compact` を使うと、自分でタイミングを選べます。関係のない別のタスクに移る場合は、`/clear` または `/new` で会話をリセットします。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者ごとに好みもあります。Copilot CLI では、モデルの一覧を確認して使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して知識を共有すると、AI ツールの活用方法をより改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートし、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合った方法を選びます。例: ローカルの Markdown ファイルに出力する `/share file`。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## 省略可能: 並行作業や委任をする

このワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクで使う次のコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分配し、並列に実行できます。
- `/worktree` は別のタスク用に分離した Git worktree を作成できます。
- `/delegate` は非同期で作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモートの作業を発生させる場合があるため、任意です。使うときは、新しい範囲の明確なタスクから始め、通常のワークフローで結果をレビューします。非同期のエージェント作業について詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部で行われていることを確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って Copilot のコンテキスト管理を確認しました。
- `/model` を使って利用できるモデルを調べました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。次は、学んだことと今後の学習方法を[振り返ります][next-lesson]。

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
