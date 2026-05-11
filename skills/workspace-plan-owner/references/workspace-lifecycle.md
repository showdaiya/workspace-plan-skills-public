# workspace lifecycle

## 目的

workspace の状態を `active` / `backlog` / `archive` で安定運用し、開きっぱなしの `active` や重複着手を防ぐための owner 向け reference です。

## 使う場面

- 既存 workspace を `active` / `backlog` / `archive` のどこに置くか判断するとき
- 完了済みまたは長期停止中の `active` workspace を閉じるか見直すとき
- 新しい workspace に着手する前に、既存 `active` の整理要否を確認するとき
- 複数 workspace を並行で進めてもよいか、例外条件に当たるか判断するとき
- archive 済みの作業を再開したいが、履歴を壊さず扱いたいとき

## 状態の基本

- `active`: いま進めている作業。短期 blocked を含む
- `backlog`: まだ着手しない、または長期待ちに戻した作業
- `archive`: 完了・cancelled・終了済みの作業

## 完了済み active を閉じるルール

次の条件を満たした `active` workspace は、開きっぱなしにせず閉じます。

- `plan.md` の完了条件が満たせた
- 未完了 Todo の扱いが `done` / `cancelled` / `blocked` として整理できた
- 主要成果物と未解決事項を `plan.md` からたどれる
- 子報告の反映が終わった
- 次回へ持ち越すなら、その理由と再開条件が書かれている

閉じる時の基本手順:

1. `plan.md` に完了ログ・終了理由・未解決事項を書く
2. 必要なら `notes.md` を更新する
3. 主要成果物一覧を更新する
4. workspace を `archive/` へ移す

## 複数 active の例外条件

1 つのセッションでは、`active` は原則 1 件を推奨します。複数 `active` は次のような例外時だけ許容します。

- ユーザーが明示的に並行進行を求めた
- 作業対象ファイル・成果物が分離され、衝突しない
- 外部待ちの短期 blocked があり、その間に別件を進める必要がある
- 明確に別テーマで、1 つに混ぜると追跡しづらい

複数 `active` にする場合、親は各 workspace について次を説明できるようにします。

- なぜ並行にするのか
- どの成果物・Todo が分離されているか
- どちらが先に閉じられる見込みか
- 同一ファイル同時編集が起きないか

## backlog に戻す判断

次のような場合は、`active` に置き続けるより `backlog` へ戻すことを検討します。

- 再開時期が未定
- 追加判断や外部待ちが長引く
- 現時点で他の workspace を優先する方が明確

戻す前に `plan.md` へ次を残します。

- いま止まっている理由
- 再開条件
- すでに終わったこと
- 次回の最初の一手

## archive からの再開

- 既存 archive を直接 `active` に戻さない
- 新しい workspace を作り、`plan.md` に参照元 archive workspace を記録する
- archive 側は履歴として読み取り中心で扱う

## 次の workspace に進む前の確認

新しい workspace を作る、または別件を進める前に次を確認します。

1. 既存 `active` に、閉じるべき完了済み workspace が残っていないか
2. 既存 `active` の blocked は短期か、`backlog` 戻し相当か
3. 同じ目的の workspace を重複作成しようとしていないか
4. 主要成果物・判断メモが既存 workspace からたどれるか
5. 新しい workspace に分ける理由があるか。既存 workspace 継続の方が自然ではないか

## owner メモ

- workspace が増えるほど、親の統合作業が増える
- 迷ったら「新規 workspace を増やす」より「既存 workspace を閉じる / 続ける」判断を先にする
- 完了済み `active` を残すと、次回再開時の判断コストが増える
