---
name: workspace-plan-owner
description: 複雑作業・複数日にまたがる作業を {WORKSPACE_ROOT} の workspace と plan.md 起点で管理する親エージェント owner/controller 用 skill。ユーザーが「作業計画」「workspace」「plan.md」「前回の続き」「archive から再開」「backlog から active」「Todo 分解」「子へ委任」「子の報告統合」「blocked 整理」などに触れたら積極的に使う。親として workspace 作成・選定・状態移動、Memory Context の必要性確認、plan 更新、子への委任、報告統合、blocked/archive 判断を行い、実作業は workspace-plan-executor に委任する。子として委任範囲だけ実行する場合は workspace-plan-executor、memory topic の保存・整理が主目的なら memory-ops、個人 ToDo 管理が主目的なら todo-manager を使う。
---

# workspace-plan-owner

## 目的

この skill は、親エージェントが `{WORKSPACE_ROOT}` 配下の workspace と `plan.md` を owner/controller として管理し、複雑作業を安全に進めるための運用手順です。

親は plan / workspace の責任者です。サブエージェントへ実作業を委任しても、作業目的、Todo 状態、実行ログ、workspace 状態、子の報告統合は親が責任を持ちます。親は executor ではありません。

## 役割

### 親エージェント owner/controller

- ユーザー依頼から目的、成功条件、対象外、停止条件を整理する
- workspace を作成・選定し、状態を管理する
- `plan.md` を作成・更新する
- Todo を分解し、状態を更新する
- 子へ委任する範囲を決め、短い委任入力を作る
- 子の報告を検査し、内容別に `plan.md` / workspace 成果物 / `notes.md` / memory 候補へ仕分ける
- blocked / backlog 戻し / archive 移動の判断を行う
- memory へ移す候補を報告する。ただし明示指示なしに memory topic は更新しない

### 親は executor ではない

親は owner/controller であり、executor ではない。
`workspace-plan-owner` を使って plan / workspace 運用に入った後、親は実作業 Todo を自分で実行しない。
実作業 Todo は必ず `workspace-plan-executor` を使う子エージェントへ委任する。

親が直接行ってよいのは、`plan.md` の作成・更新、Todo 分解、委任入力作成、子報告の検査と統合、workspace 状態判断、ユーザー返答などの owner-control 作業に限る。

実作業 Todo とは、調査、実装、検証、テスト、外部調査、文書本文作成、設計案作成、比較・分析、成果物作成など、Todo の完了条件を満たすための実質的な作業を指す。

迷った場合は executor-work とみなし、子へ委任する。

### 子エージェント executor/reporter

子は委任された Todo 範囲だけを実行し、結果を親へ報告します。子に `plan.md` の直接更新、workspace 移動、memory topic 更新を任せません。

## 使う場面

- 複数 Todo に分解する必要がある作業
- 複数セッションにまたがる、または前回の続きとして再開する作業
- 調査、設計、実装、テスト、文書化などが混ざる作業
- サブエージェントへ作業範囲を委任する可能性がある作業
- 判断根拠や中間成果物を workspace に残したい作業
- `{WORKSPACE_ROOT}/active/`、`backlog/`、`archive/` の状態確認・移動が必要な作業
- 既存 workspace の `plan.md` を読み、次の Todo を進める作業

## 使わない場面

- 1 回の短い回答で終わる質問
- 1 ファイルの軽微な修正など、workspace を作るほどではない単発作業
- memory topic の保存・整理そのものが主目的の作業。その場合は `memory-ops` を使う
- Markdown 永続 ToDo の個人タスク管理が主目的の作業。その場合は `todo-manager` を使う
- 子として委任範囲を実行するだけの場面。子用 skill がある場合はそちらを使う

## 基本原則

- 親は owner/controller として、workspace / plan / Todo / 委任 / 統合を管理する
- 実作業 Todo は executor-work として子へ委任し、親は代行しない
- `task type` は親が選び、子は選ばれた type と必要な `extra task types` から読む task reference を判断する
- `plan.md` は進行管理、`requirements.md` は要件定義の初期正本、workspace 成果物は内容の正本、`notes.md` は非正本の一時メモ、memory は長期知識として分ける
- 再開や委任で参照価値がある設計、方針、判断、調査、検証、タスク補助情報は、`references/artifact-candidates.md` を参照して workspace 成果物として積極的に残す
- 成果物本文の作成・更新は原則 executor-work として子へ委任し、親は管理者として成果物候補の選定、許可範囲の指定、情報提供、報告検査、要約統合、必要時の rename / move 判断に徹する
- 詳細手順はこの SKILL.md に重複させず、該当 reference を正本にする

## reference routing

SKILL.md は owner の原則と routing だけを持ちます。詳細手順は必要な reference を読んで実行します。

| 参照先 | 読む場面 | 責務 |
| --- | --- | --- |
| `references/plan-creation.md` | 新しい workspace と初期 `plan.md` / `requirements.md` を作るとき | plan 作成前の確認、workspace 作成、template 利用、初期 plan / requirements 作成 |
| `references/workspace-lifecycle.md` | workspace の作成・移動・archive・backlog 判断をするとき | workspace 状態管理 |
| `references/delegation-template.md` | 子へ委任するとき | 親による `task type` 選択、`extra task types` 整理、委任入力作成 |
| `references/report-integration-checklist.md` | 子の報告を受けたとき | 報告検査、仕分け、統合チェック |
| `references/plan-artifact-tracking.md` | 主要成果物を plan へ残すとき | 成果物索引の管理 |
| `references/artifact-candidates.md` | workspace 成果物の作成・更新候補を確認するとき | 成果物候補、作成・更新タイミング、候補一覧 |
| `references/workspace-notes-policy.md` | `notes.md` へ残すか迷うとき | 一時メモと正式成果物の境界 |

## owner flow

1. 目的、成功条件、対象外、停止条件を整理する
2. workspace が必要か、既存 workspace を使うか判断する
3. 新しい workspace と初期 `plan.md` を作る場合は `references/plan-creation.md` に従い、既存 workspace を使う場合は `plan.md` を読む
4. Todo を `T1`, `T2` のように分解し、owner-control / executor-work に分ける
5. 設計、方針、判断、調査、検証、タスク補助情報のうち、再開や委任で参照価値があるものを workspace 成果物候補として整理する
6. executor-work は親が実行せず、`references/delegation-template.md` に従って子へ委任する
7. 子の報告を受けたら `references/report-integration-checklist.md` に従って検査する
8. `plan.md`、workspace 成果物、`notes.md`、memory 候補へ仕分けて統合する
9. 完了・blocked・backlog 戻し・archive を判断する

## workspace / plan 原則

- workspace root は `{WORKSPACE_ROOT}` 固定
- 状態は `backlog` / `active` / `archive` の親ディレクトリで表す
- plan 本体は常に `plan.md`
- 新規 workspace の最小構成は原則 `plan.md` と `requirements.md` とし、README は自動作成しない
- memory の参照・保存・更新が必要な場合は `memory-ops` を使う
- memory topic はユーザーの明示指示なしに作成・更新・削除しない

workspace lifecycle は `references/workspace-lifecycle.md`、成果物索引は `references/plan-artifact-tracking.md`、notes の境界は `references/workspace-notes-policy.md` を正本にします。

## delegation 原則

親が `task type` を選びます。子は選ばれた `task type` と必要な `extra task types` から task reference を判断し、委任範囲だけを正確に実行します。

- `task type` の種類、選択基準、委任テンプレートは `references/delegation-template.md` に集約する
- 子には対象 Todo、完了条件、受入条件、検証方法、許可成果物、禁止事項、停止条件を具体的に渡す
- 共通ルール長文は毎回貼らず、`task type` と子側固定ルールに任せる
- 複合タスクでは主 task type を 1 つ選び、補助観点を `extra task types` にする
- 同一ファイルを複数子へ同時編集させない
- 子に `plan.md` 更新、workspace 状態移動、memory topic 更新を任せない

## integration 原則

子の報告はそのまま貼らず、親が検査してから統合します。

統合時の仕分けと確認項目は `references/report-integration-checklist.md` を正本にします。

## stop conditions

次の場合、親は委任または統合を止めてユーザーに確認します。

- 要件、優先順位、受入条件が曖昧
- 破壊的・不可逆な変更を含む
- 権限、費用、外部サービス、秘密情報が関わる
- 子へ安全に渡せる完了条件・停止条件を作れない
- executor-work を子へ委任できない

子へ委任できない executor-work を、親が代行して進めてはいけません。

## prohibited

- workspace root を `{WORKSPACE_ROOT}` 以外へ戻す
- plan 本体名を `plan.md` 以外にする
- plan ファイル名で状態管理する
- 親が実作業 Todo を直接実行する
- 子の報告なしに実作業 Todo を `done` にする
- 子に `plan.md`、workspace 状態、memory topic を更新させる
- `notes.md` を正式成果物の代替にする
- 新規 workspace に README を自動作成する
- ユーザーの明示指示なしに memory topic を作成・更新・削除する
- archive workspace を直接 active へ戻す
- blocked 専用ディレクトリを作る
- 親が明示していない削除・リネーム・移動を子に実行させる
- 複数子へ同一ファイルを同時編集させる

## completion report

ユーザーへの完了報告はします。
もし他フォーマットぶつかる場合は、文頭に完了フォーマットを簡潔に残し、その後他フォーマットで記載する。

```text
## 完了報告

- 実施内容:
- 更新したファイル:
- 確認結果:
- 未解決・次アクション:
```
