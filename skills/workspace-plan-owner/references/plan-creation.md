# plan creation

## 目的

親エージェントが、新しい workspace と初期 `plan.md` / `requirements.md` を安定して作成するための reference です。

`workspace-plan-owner` は plan 作成責務を持ち続けますが、詳細手順はこの reference に寄せます。`plan-template.md` は plan 書式の正本、`requirements-template.md` は requirements 書式の正本として扱い、この reference に細かな見出し構成を重複させません。

## 目的と責務境界

| 参照先 | 役割 | この reference との境界 |
| --- | --- | --- |
| `workspace-plan-owner/SKILL.md` | owner の原則、routing、owner flow | plan 作成責務を持つ。詳細手順はここへ寄せる |
| `references/plan-creation.md` | 新規 workspace / 初期 `plan.md` / 初期 `requirements.md` 作成手順 | plan 作成前後の判断と進め方を扱う |
| `/agent_work/workspaces/_template/README.md` | workspace テンプレート置き場の案内 | 手順の正本ではない。新規 workspace へコピーしない |
| `/agent_work/workspaces/_template/plan-template.md` | `plan.md` の書式正本 | 見出し、項目名、記載欄の正本。作成手順はここに寄せない |
| `/agent_work/workspaces/_template/requirements-template.md` | `requirements.md` の書式正本 | ユーザー要件の初期正本。要件のみを置き、設計意図や判断過程は混ぜない。作成手順はここに寄せない |
| `references/artifact-candidates.md` | workspace 成果物候補 | plan 作成時や作業中に、どの成果物を作成・更新するか判断するための候補一覧 |
| `references/workspace-lifecycle.md` | `active` / `backlog` / `archive` の状態管理 | 初期配置の判断だけ触れる。移動・終了運用は lifecycle を正本にする |
| `references/delegation-template.md` | 子への委任入力作成 | 初期 plan に委任前提の情報を置く。委任文面の正本は delegation |
| `references/workspace-notes-policy.md` | `notes.md` の役割と仕分け | 初期 plan 作成手順では `notes.md` を正本にしない |
| `references/plan-artifact-tracking.md` | plan 内の主要成果物一覧管理 | 初期 plan に成果物欄は置けるが、一覧の管理ルールは別 reference |
| `memory-ops` | memory の参照・保存・更新ルール | memory が必要かの判断だけ行い、詳細手順は memory-ops を使う |

## 使う場面

- 新しい複雑作業を始めるために workspace を新規作成するとき
- 前回の続きだが、archive を直接戻さず新規 workspace で再開するとき
- ユーザー依頼を複数 Todo に分けて、owner-control / executor-work を整理したいとき
- 子へ委任する前提で、初期 `plan.md` の土台を作りたいとき
- plan 作成時点で、ユーザーが理想とする要件を `requirements.md` に切り出したいとき

次の場面は、この reference 単体ではなく他の正本を優先します。

- 既存 workspace の状態移動や終了判断: `references/workspace-lifecycle.md`
- 子への委任入力作成: `references/delegation-template.md`
- memory topic の参照・保存・更新: `memory-ops`
- `plan.md` の見出しや項目名の確認: `/agent_work/workspaces/_template/plan-template.md`
- `requirements.md` の見出しや項目名の確認: `/agent_work/workspaces/_template/requirements-template.md`
- 成果物候補の確認: `references/artifact-candidates.md`

## plan 作成前のヒアリング項目

新規 plan を作る前に、少なくとも次を確認します。

1. 何を成果物として完成させたいか
2. 最終的な目的は何か
3. 成功条件は何か
4. 対象外は何か
5. 停止条件は何か
6. どのファイル・場所・システムが関係するか
7. 破壊的操作、外部影響、秘密情報、権限の制約があるか
8. 1 回で終わるか、複数セッションや委任を前提にするか
9. ユーザーが理想とする要件、必須要件、できれば満たしたい要件、避けたいことは何か
10. 完了までに必要そうな章立てや Todo のまとまりは何か
11. 既存 workspace の継続ではなく、新規 workspace に分ける理由があるか

不足がある場合は、plan を書き始める前にユーザーへ確認します。推測で成功条件や対象外を埋めません。

## memory 確認要否の判断

memory 確認は常に必須ではありません。次の場合にだけ検討します。

- ユーザーが「前回の続き」「前に決めた方針」など継続文脈を示した
- 同種の運用方針や過去判断を引き継がないと、初期 plan がぶれやすい
- archive workspace 参照だけでは足りず、長期的な判断理由を確認したい

不要な例:

- 単発の新規作業で、継続文脈がない
- ローカルの workspace や依頼文だけで十分に plan を作れる

memory を確認する場合も、詳細手順は `memory-ops` を正本にします。

- 読むときは `memory/index.md` を入口にする
- 必要な topic だけを読む
- ユーザーの明示指示なしに memory topic を保存・更新しない

## workspace の作成先と命名

workspace root は常に `/agent_work/workspaces/` です。

初期配置の基本:

- すぐ着手する plan: `/agent_work/workspaces/active/`
- まだ着手せず待機させる plan: `/agent_work/workspaces/backlog/`
- 新規作成時に `/archive/` 配下へ置かない

状態運用の詳細や移動判断は `references/workspace-lifecycle.md` を参照します。

命名の基本:

- ディレクトリ名は `YYYY-MM-DD-題名`
- 題名は短く、目的が追える語を使う
- plan ID も同系統の文字列にそろえると追跡しやすい
- 同目的の既存 workspace がある場合は重複作成しない

## `plan-template.md` の使い方

`/agent_work/workspaces/_template/plan-template.md` は、`plan.md` の書式正本です。

使い方:

1. 見出し構成と必須項目を template で確認する
2. 対象 workspace に `plan.md` を作成する
3. template の項目名を維持しつつ、今回の作業内容に合わせて埋める
4. 章立てや Todo 数は作業に合わせて増減してよいが、`plan.md` 本体・workspace root・状態表現の原則は崩さない

注意:

- `plan-template.md` 自体を plan 本体として使わない
- template を移動・削除しない
- この reference に template 全文を書き写さない
- 細かな書式判断は template を見て合わせる

## `requirements-template.md` の使い方

`/agent_work/workspaces/_template/requirements-template.md` は、`requirements.md` の書式正本です。

plan 作成時点では、ユーザーが理想とする要件はすでに依頼文やヒアリング結果に含まれている前提で扱います。親はその時点で `requirements.md` を作成し、plan の進行管理とは分けて、要件の初期正本として残します。

使い方:

1. 見出し構成と必須項目を template で確認する
2. 対象 workspace に `requirements.md` を作成する
3. ユーザーが明示した要件を、理想状態、必須要件、任意要件、対象外、受入条件へ分けて記載する
4. 推測で要件を補完しない。不明点は `未確定・確認事項` に置く
5. `plan.md` の成果物欄と主要成果物一覧から `requirements.md` をたどれるようにする

注意:

- `requirements.md` は、ユーザー要件の正本として扱う
- `requirements.md` には要件のみを置く。「なぜそう決めたか」「どの案を却下したか」「会話中の判断過程」などは置かない
- `plan.md` には要件全文や判断過程を重複させず、進行管理に必要な要約と参照先だけを置く
- 要件確定までの設計意図、案の変遷、採用・不採用理由は、必要に応じて `design-decision-record.md` や `decision-log.md` などへ分離する
- `notes.md` に要件定義の本体を置かない
- requirements 作成は owner-control 作業として親が行う

## 初期 `plan.md` 作成ステップ

1. ユーザー依頼から、目的、成功条件、対象外、停止条件、成果物候補を整理する。設計、方針、判断、調査、検証、タスク補助情報を workspace 成果物として残す必要がある場合は、`references/artifact-candidates.md` を参照する
2. 既存 workspace 継続か新規作成かを判断する。新規作成が妥当なら初期配置先を `active` / `backlog` から決める
   - 要件確定までの判断、案の変遷、採用・不採用理由、設計意図を後から参照する必要がある場合は、`requirements.md` や `plan.md` へ混ぜず、`design-decision-record.md` または `decision-log.md` などの正式成果物へ切り出す
3. `YYYY-MM-DD-題名` で workspace ディレクトリを作成する
4. `plan-template.md` を参照し、workspace 配下に `plan.md` を作成する
5. `requirements-template.md` を参照し、workspace 配下に `requirements.md` を作成する
6. `## 基本情報` を埋める
   - 計画ID
   - 作成日 / 更新日
   - 対象
   - 最終的な目的
   - 成功条件
   - 成果物
   - 前提条件
   - 対象外
   - 開始条件
   - 停止条件
   - 関係ファイル/場所
   - workspace root
   - ワークスペース
   - 必要時のみ `参照元 archive workspace`
7. `requirements.md` に、ユーザーが理想とする要件、必須要件、任意要件、対象外、受入条件、未確定・確認事項を記載する
   - `requirements.md` は要件の正本に限定し、設計意図や判断過程は必要に応じて `references/artifact-candidates.md` の成果物候補へ分離する
8. `## 全体の禁止事項` と `## 全体の補足` に、今回の制約や判断メモを必要最小限で記載する
9. 作業を章ごとに分け、Todo を `T1`, `T2` のように分解する
10. 各 Todo に少なくとも `状態`、`完了条件`、`参照範囲`、`メモ` を置く
11. Todo ごとに owner-control と executor-work を分ける。executor-work は親が直接実行しない前提で書く
12. 委任が想定される場合、子へ渡す時に必要な完了条件、成果物指定、制約、検証観点を plan 上でもたどれる状態にする
13. `## 実行ログ` に plan 初版作成と `requirements.md` 初版作成を記録する

新規 workspace 作成時の注意:

- `/agent_work/workspaces/_template/README.md` は template 置き場の案内であり、新規 workspace へコピーしない
- 新規 workspace の最小構成は原則 `plan.md` と `requirements.md` とする
- workspace 用 README が必要な場合だけ、親が目的、読み手、記載内容を明確にした成果物として別途作成する

## 作成後の確認と次アクション

初期 `plan.md` 作成後は次を確認します。

- workspace パスが `/agent_work/workspaces/{active|backlog}/...` 配下にあるか
- plan 本体名が `plan.md` になっているか
- 要件正本が `requirements.md` になっているか
- `plan-template.md` の原則に反していないか
- `requirements-template.md` の原則に反していないか
- 成功条件、対象外、停止条件が空欄や推測のまま残っていないか
- ユーザーが理想とする要件が `requirements.md` に分離され、plan 側へ全文重複していないか
- Todo が実行可能な粒度か
- owner-control と executor-work の境界が見えるか
- memory を読む必要がある場合だけ、`memory-ops` を使う前提になっているか

次アクションの基本:

- すぐ進める owner-control 作業があるなら親が続ける
- executor-work があるなら `references/delegation-template.md` に従って子へ委任する
- 主要成果物を plan で追跡したくなったら `references/plan-artifact-tracking.md` を使う
- 追加成果物の作成・更新で迷ったら `references/artifact-candidates.md` を使う
- 成果物本文の作成・更新が必要なら、原則として `references/delegation-template.md` に従って子へ委任する
- 一時メモが増えたら `references/workspace-notes-policy.md` に従って `notes.md` との境界を保つ

## owner メモ

- plan 作成責務は `workspace-plan-owner` に残る
- 詳細手順はこの reference に寄せる
- plan 書式の正本は常に `/agent_work/workspaces/_template/plan-template.md`
- requirements 書式の正本は常に `/agent_work/workspaces/_template/requirements-template.md`
- memory の詳細運用は `memory-ops` を正本にする
