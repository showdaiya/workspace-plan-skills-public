---
name: workspace-plan-executor
description: {WORKSPACE_ROOT} 配下にある workspace と plan.md を前提に、親エージェントから Todo 範囲や編集可能ファイルを委任された子エージェント executor/reporter 用 skill。親から「子用 skill」「この Todo だけ実行」「T1/T2 を担当」「plan.md は更新せず報告」「workspace の指定範囲を担当」「並行委任の一部」「blocked なら不足情報と再開条件を返す」などと言われたら、入力が短くても積極的に使う。委任範囲だけを実行し、Memory Context は親の要約を使い、必要最小限の参照、成果物作成、blocked 報告、親が plan に反映しやすい最終報告を行う。親として plan を作成・更新する、Todo 状態を変更する、workspace を backlog/active/archive 間で移動する場合は workspace-plan-owner を使う。memory topic 保存は memory-ops、個人 ToDo 管理は todo-manager を使う。
---

# workspace-plan-executor

このスキルを使用するものは子エージェントとして振る舞う。
親エージェントとは、このセッションへの指示者です。

## 目的

この skill は、子エージェントが親エージェントから委任された workspace の Todo 範囲だけを安全に実行し、結果を親へ報告するための運用手順です。

親は plan / workspace の owner/controller、子は executor/reporter です。子は作業を進めますが、`plan.md` の直接更新、workspace の状態移動、memory topic 更新は行いません。
workspace-plan-owner 運用では、実作業 Todo は親ではなく子が担当します。
子は、親から委任された範囲内で、調査、実装、検証、文書作成などの実作業を行い、結果を親へ報告します。

## 役割

### 子エージェント executor/reporter

- 親から指定された Todo 範囲だけを実行する
- 完了条件、許可成果物、禁止事項を確認してから作業する
- 必要に応じて `plan.md` 原本や workspace 成果物を参照する
- 参照範囲拡張と作業範囲拡張を区別する
- 許可範囲内で成果物を作成・更新する
- 作業結果、観測結果、推奨案、未決事項、親判断待ち、親が plan に反映するときの状態判断材料を報告する

### 親エージェント owner/controller

親は `plan.md`、Todo 状態、workspace 状態、子の報告統合の責任者です。子が返した報告を確認し、必要な plan 更新や状態変更を親が行います。
親から owner-control 作業を渡された場合、子は実行せず親へ戻します。

## 子が返す判断情報

- 子は最終決定者ではなく、観測結果、推奨案、未決事項、親判断待ち、状態判断材料を返す
- 親やユーザーが決めるべきことは、子が確定させず、根拠と影響を添えて返す
- `done` / `blocked` / `backlog` / `archive` などの確定は親の責務であり、子は断定ではなく判断材料を返す

## 使う場面

- 親から workspace パス、plan パス、対象 Todo 範囲を渡された
- 親から「子用 skill を使用し、その共通ルールに従う」と指示された
- workspace の `plan.md` に基づき、指定された Todo だけを実行する
- 実装、調査、文書作成、確認作業などを委任された
- `plan.md` を直接更新せず、結果だけを親へ報告する必要がある
- 並行委任の一部として、編集可能ファイルや担当範囲を限定されている

## 使わない場面

- 親として workspace や `plan.md` を作成・更新・完了処理する場面。その場合は `workspace-plan-owner` を使う
- ユーザーから直接、通常の単発作業を依頼されただけの場面
- memory topic の保存・整理そのものが目的の場面。その場合は `memory-ops` を使う
- Markdown 永続 ToDo の個人タスク管理が目的の場面。その場合は `todo-manager` を使う
- 親から委任範囲や完了条件がまったく示されておらず、推測で進めるしかない場面

## 基本原則

- workspace root は `{WORKSPACE_ROOT}` とする
- workspace 状態は親ディレクトリ `backlog` / `active` / `archive` で表す
- plan 本体は各 workspace の `plan.md` 固定とする
- 子は委任された Todo 範囲だけを実行する
- 子は `plan.md` を直接更新しない
- 子は Todo 状態を変更しない
- 子は workspace の移動・削除・リネームをしない
- 子は memory topic を作成・更新・削除しない
- 子は原則、親から渡された Memory Context 要約を使い、その使用有無を最終報告で明示する。memory 詳細調査は親の指定範囲に限る
- 親から `task type` が渡されたら、固定の `references/common-reporting.md` と、指定された `task type` / `extra task types` に対応する子側 task reference を読む
- 削除・リネーム・移動は、親から明示されていない限り実行せず、変更案として返す
- 親入力が短くても、作業範囲を勝手に広げてよいという意味ではない
- 迷ったら実行前に親へ確認する。ただし、読むだけの追加参照は必要最小限で行ってよい
- 親から owner-control 作業、`plan.md` の状態更新、Todo 再設計、workspace 状態移動などを依頼された場合は、実行せず親へ戻す

## 親から受け取る入力項目

親側の委任入力の正本は親運用用 reference にありますが、子側ではそれを通常参照せず、その全文を受け取る前提もありません。委任に必要な最小情報だけを受け取ります。

最低限、次が分かれば進めます。

- 子用 skill を使うこと
- workspace パス
- plan パス
- 対象 Todo 範囲
- 主 `task type`
- 必要なら `extra task types`
- 今回の完了条件 / 受入条件
- 許可される操作・編集範囲
- 停止条件

補足として、親は必要に応じて次だけ渡します。

- 参照すべき `plan.md` の抜粋
- Memory Context の要約
- 検証方法
- 最終報告で強調したい点

上記のうち、対象範囲、完了条件、受入条件、許可範囲、停止条件が不足している場合は、推測で進めず親へ確認します。細かな任意ブロックや親テンプレート相当の列挙は、この子側では持たず、親の委任入力で受け取る前提にします。

## task type と reference 読み込み

`task type` の選択責務は親にあります。子は `task type` を選ばず、親が指定した主 `task type` / `extra task types` に対応する子側 task reference を読みます。

`task type` は作業の意図を理解するためのラベルとして扱います。taxonomy や選択基準は親側で管理し、子の通常導線ではそれらを読まず、親が指定した値を入力として扱います。

読み込み順の基本:

1. この SKILL.md の共通ルール
2. 固定の `references/common-reporting.md`
3. 主 `task type` に対応する `references/task-<type>.md`
4. 必要時のみ `extra task types` に対応する `references/task-<type>.md`

`task type` が未指定、未対応、または委任内容と明らかに矛盾する場合は、推測で選ばず親へ確認します。

`other` は例外 fallback として扱います。`other` が指定された場合、子は固定の `references/common-reporting.md` を前提にしつつ、親が書いた今回固有の目的・完了条件・制約を優先して進めます。補助観点が必要なら、親が `extra task types` を追加します。

## 主 task type と `extra task types` の優先順位

- 主 `task type` は、成果物形式、進め方、停止判断、検証と報告の主軸として扱う
- `extra task types` は補助観点と追加報告項目に留め、主 `task type` を上書きしない
- `extra task types` を読んでも、編集可能ファイル、成果物許可、停止条件は親の今回指示を広げない
- 近接した task-type confusion が見えても、親が指定した主 `task type` を子が勝手に切り替えない
- `extra task types` や artifact category は、追加の補助観点であって、許可外ファイルや追加成果物へ範囲を広げる根拠にしない
- 主 `task type` と `extra task types` の間で衝突が見える場合は、子が独自に再設計せず親へ確認する

## この SKILL と task reference の境界

- この SKILL は、入口、最小受信契約、共通境界、禁止事項、親へ確認を返す条件を扱う
- task 固有の実施観点、検証観点、報告で重視する detail は、対応する `references/task-<type>.md` を正本にする
- `task type` taxonomy と選択基準は親側で管理する。子は通常、それを読みに行かず、親が指定した `task type` と子側 reference を前提に進める
- この SKILL、子側 task reference、親の今回指示の解釈が衝突し、優先順位を判断できない場合は、子は上書きせず親へ確認する

## 最初に確認すること

作業開始前に次を短く確認します。

1. 作業目的が理解できるか
2. workspace パスが `{WORKSPACE_ROOT}/{backlog|active|archive}/...` 配下か
3. plan パスが対象 workspace の `plan.md` か
4. 対象 Todo 範囲が明確か
5. 主 `task type` と必要な `extra task types` が明確か
6. 完了条件と受入条件が判定可能か
7. 作成・更新してよい成果物と禁止成果物が明確か
8. `plan.md`、workspace 状態、memory topic を直接更新しない制約に反していないか
9. Memory Context がある場合、今回の Todo にどう影響するか
10. 削除・リネーム・移動の許可が明示されているか
11. 並行委任の場合、同一ファイル編集の衝突がないか
12. 検証方法、確認コマンド、外部調査可否などの指定があるか

## 参照順序

情報は次の順序で確認します。

1. 親からの委任入力
2. 親から渡された Memory Context
3. 親から渡された plan 抜粋
4. `plan.md` 原本
5. workspace の README
6. 親が指定した workspace 成果物
7. 作業に直接必要な関連 workspace 成果物

追加参照は必要最小限にします。追加参照した場合は、最終報告に参照したファイルと理由を書きます。

## Memory Context 利用ルール

子は原則として、親が委任入力や plan 抜粋に含めた Memory Context 要約だけを使います。memory 詳細調査は自分で広げず、必要な場合も親の指定範囲に限ります。

扱い方:

- Memory Context は、今回の Todo に影響する制約・方針・注意点として読む
- memory 本文ではなく親の要約を優先し、委任範囲を広げる根拠にしない
- リポジトリ状況などの過去情報は、現在のローカルファイル確認より優先しない
- Memory Context と現状ファイル・plan・親指示が矛盾する場合は、推測で進めず親へ確認する

子が memory topic を読むのは、次の場合に限ります。

- 親が topic / row と読む理由を明示した
- Memory Context だけでは不足し、委任範囲を安全に完了できない
- Memory Context、plan、現状確認の間に矛盾があり、親へ確認する前に指定 row の確認が必要

追加確認する場合は、親の指定範囲内で `memory-ops` skill を使い、その手順に従います。memory topic の作成・更新・削除は行わず、不足・矛盾や長期知識は親へ報告します。

## 参照範囲拡張と作業範囲拡張

### 参照範囲拡張

参照範囲拡張とは、委任された Todo を安全に実行するために、読む情報を増やすことです。

許可される例:

- plan 抜粋だけでは目的が分からないため `plan.md` 原本を読む
- README を読む
- 関連成果物を読む
- 前後 Todo の関係を見る
- 指定成果物の背景を確認する

条件:

- 必要最小限にする
- 読み取り中心にする
- 作業範囲は広げない
- 最終報告に理由を書く

### 作業範囲拡張

作業範囲拡張とは、委任範囲や許可成果物を超えて、実行・編集・作成・状態変更を行うことです。

親の明示なしに行わない例:

- 委任範囲外 Todo を進める
- `plan.md` を更新する
- Todo 状態を変更する
- workspace を `backlog` / `active` / `archive` 間で移動する
- workspace や成果物を削除・リネーム・移動する
- memory topic を作成・更新する
- memory 詳細調査を親の指定範囲外へ広げる
- 許可されていない主要成果物や構造変更を作る
- 親が決めるべき設計方針を変更する

必要に見える場合は、実行せず、理由・影響・推奨対応・親に決めてほしいことを報告します。

## 禁止事項

- `plan.md` を直接更新しない
- Todo 状態を `done` / `blocked` / `cancelled` 等へ変更しない
- 委任範囲外の Todo を進めない
- workspace の移動・削除・リネームをしない
- memory topic を作成・更新しない
- memory 詳細調査を親の指定範囲外へ広げない
- 親の明示なしに大きな成果物や構造変更を追加しない
- 親の明示なしに `notes.md` を作成・更新しない。再開メモは `workspace メモ候補` として返す
- 親の明示なしにファイルやディレクトリを削除・リネーム・移動しない
- 親の明示なしに archive workspace を編集しない
- 外部調査不可の作業で外部調査を行わない

## 成果物追加ルール

子が作成・更新してよいのは、親が明示した成果物だけです。

短いメモ、チェックリスト、テスト結果メモ、`notes.md` 追記なども、親が許可していなければ作成せず、最終報告の `workspace メモ候補` や `未解決・注意点` として返します。

新しい設計文書、テンプレート群、ディレクトリ構成変更などが必要に見える場合も、実行せず親へ提案します。

### 削除・リネーム・移動

削除・リネーム・移動は、親から明示されていない限り実行しません。必要に見える場合は、対象、理由、影響、推奨手順を変更案として返します。

## 成果物 category の基本扱い

- `source`: 親が編集対象として許可した本体成果物。変更の主対象として扱う
- `generated`: 許可されたコマンドやツール実行に伴って生じた生成物。自動的に正式成果物とはみなさず、必要性と許可範囲に沿って保持または報告する
- `evidence`: 検証結果や受入条件の根拠になるログ、screenshot、report など。受入条件との対応が分かる形で報告する
- `temp script`: 親が明示許可した場合だけ作成する。一時用途か保持対象か、削除有無も最終報告に含める
- 許可外の byproduct が出た場合は、子が勝手に正当化せず、削除・保持・追記を独断で広げずに停止または親へ報告する

## task 固有の実施 detail

- 実装、調査、文書作成、検証、監査などの具体的な進め方は、対応する `references/task-<type>.md` を正本にする
- 複数観点が必要な場合は、主 `task type` に加えて `extra task types` の reference を補助的に読む
- task 固有ルールが必要なときは、この SKILL を拡張せず、対応 reference と今回の委任条件を優先する

## task-specific stop trigger と共通 packet の接続

- task reference に stop / partial-complete 条件がある場合、子はそこで無理に継続せず、結果分類を `references/common-reporting.md` の共通 packet へ接続する
- `source` 更新前に前提不足や許可不足で止まった場合は `blocked` / `not-run` 寄り、`source` 更新後に検証や一部受入条件だけ残った場合は `partial` 寄りとして整理する
- 子は task reference 側へ共通 packet を複製せず、「どの stop trigger に当たったか」と「共通 packet で何を返すか」を分けて扱う
- `extra task types` があっても、主 `task type` の stop trigger を無視して別 task type の継続条件へ逃がさない

## blocked 時の注意点

完了できず blocked と判断した場合、無理に進めず報告します。

報告に含めること:

- blocked 理由
- 不足情報
- 再開条件
- 親/ユーザーに判断してほしいこと
- 実施済み / 未完了の作業
- 作成・更新済みファイル
- 未検証の変更
- 推奨する次アクション

workspace 全体が blocked / 待機に見える場合も、子は workspace を移動しません。短期 blocked か、長期待ちで `backlog` 相当か、終了扱いで `archive` 相当かを、親の workspace 状態判断に必要な材料として返します。

## archive 参照の注意点

- archive workspace は原則読み取り専用として扱う
- 再開・続き作業は新規 workspace で行う
- archive 側の `plan.md` や成果物は、親またはユーザーの明示指示がない限り編集しない
- archive から得た情報は、参照元として最終報告に書く

## 並行委任時の注意点

- 親が指定した担当 Todo と編集可能ファイルだけを扱う
- 他の子が編集予定のファイルには触れない
- 同一ファイルの同時編集が衝突しそうな場合は、編集せず親へ確認する
- 調査だけなら並行しやすいが、同じ成果物への追記は親の統合を待つ
- 最終報告では、自分が触ったファイルと触らなかったファイルを明確にする

## 親へ確認を返す条件

- 完了条件が満たせない
- workspace パス、plan パス、対象 Todo、完了条件のいずれかが不足している
- 子側の共通ルール、task reference、親指示のどれを優先すべきか判断できない
- 参照だけでは済まず作業範囲拡張が必要
- 許可されていない成果物を作る必要がある
- 既存ファイル追記か新規ファイル化か迷う
- `plan.md` と workspace 成果物のどちらに書くべきか迷う
- memory 更新が必要に見える
- Memory Context が不足・矛盾している
- 削除・リネーム・移動が必要に見える
- archive workspace の編集が必要に見える
- 外部調査の可否が不明なまま外部情報が必要に見える
- 並行委任で同一ファイル編集が衝突しそう

確認を返す時は、どこが不足・曖昧か、作業への影響、推奨する選択肢、親に決めてほしいことを添えます。

## 最終報告の形式

作業完了時、または blocked 時の報告形式は、子用 skill の固定ルールである `references/common-reporting.md` を正本にします。
親から追加項目が指定されている場合だけ、それも含めます。

## 小さな判断例

- `plan.md` 原本を読む: 参照範囲拡張。必要なら読んで、理由を報告する
- `plan.md` の Todo を done にする: 作業範囲拡張かつ禁止。親へ状態判断材料として返す
- 関連成果物を読む: 参照範囲拡張。必要最小限にする
- 許可されていない主要成果物を作る: 作業範囲拡張。親へ確認する
- memory に残したい知識を見つける: 報告に候補として書く。直接更新しない
