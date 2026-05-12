# delegation template

## 目的

親が子へ委任する時の routing と入力形式を定める reference です。

親は主 `task type` を選び、必要なら補助観点として `extra task types` を指定します。
子は選ばれた `task type` / `extra task types` を読み、対応する task reference を判断して委任範囲だけを実行します。

## 使う場面

- 子へ委任する Todo 範囲を切り出し、委任入力を作るとき
- 主 `task type` を 1 つ選び、必要なら `extra task types` を決めるとき
- 完了条件、受入条件、検証方法、成果物指定を子へ具体的に渡したいとき
- 設計、方針、判断、調査、検証、タスク補助情報の本文作成・更新を子へ委任したいとき
- 複合タスクを、主 task type と補助 task type に分けて安全に委任したいとき

## 責務分離

| 役割 | 責務 |
| --- | --- |
| 親 | Todo を分解し、主 `task type` を選び、必要なら `extra task types` と完了条件を指定する |
| 子 | 指定された `task type` / `extra task types` から読む task reference を判断し、完了条件・受入条件・停止条件に従って実行する |

子に `task type` を選ばせません。
`task type` が未指定、または作業内容と明らかに矛盾する場合、子は推測で進めず親へ確認します。
そのため、正確な `task type` を選択してください。

## task type 選択基準

主成果物と主問いで 1 つ選びます。迷う場合は「何を完成させる Todo か」を優先します。

| task type | 主成果物 / 主問い | 選ぶ場面 | 対応 task reference |
| --- | --- | --- | --- |
| `research` | 判断材料 / 何が分かったか | read-only 調査、既存実装確認、比較、原因調査 | `references/task-research.md` |
| `design` | 方針・設計判断 / どう作るべきか | 方針整理、基本設計、代替案比較、アーキテクチャ検討 | `references/task-design.md` |
| `implementation` | 動く変更 / 何を変更するか | コード実装、修正、機能追加、実装に伴うテスト作成 | `references/task-implementation.md` |
| `verification` | 検証結果 / 正しく動くか | テスト実行、再現確認、品質確認、動作検証 | `references/task-verification.md` |
| `documentation` | 読むための文書 / どう説明するか | README、手順書、利用者向け説明、既存設計の文書化 | `references/task-documentation.md` |
| `refactor-cleanup` | 構造整理 / どう片付けるか | rename / move、不要物除去、構成見直し、責務整理 | `references/task-refactor-cleanup.md` |
| `review-audit` | 指摘・監査結果 / 何が危ないか | コードレビュー、設計レビュー、リスク洗い出し、実装前後の監査 | `references/task-review-audit.md` |
| `data-config` | データ・設定差分 / 何を設定するか | JSON / YAML / CSV / SQL / 設定ファイル / データ差分 | `references/task-data-config.md` |
| `communication-summary` | 伝達文 / 何を相手に伝えるか | 要約、報告文、引き継ぎ文、質問整理、ユーザー向け説明 | `references/task-communication-summary.md` |
| `other` | 親が定義 | 上記に収まらない例外 fallback。親が今回固有の進め方を委任入力で明示する | なし（必要なら `extra task types` で補助） |

## 選択手順

1. Todo の主成果物を確認する
2. 主成果物に合わせて `task type` を 1 つ選ぶ
3. 実装 + 検証など複合タスクなら、主 type を 1 つにし、補助観点を `extra task types` に入れる
4. 迷った場合は、子に選ばせず親側で分割する

迷いやすい場合の優先規則:

- 設計を決めるなら `design`、決まった内容を読みやすく文書化するなら `documentation`
- 事実を集めるなら `research`、良し悪しを判定して指摘するなら `review-audit`
- コードやテストを変えるなら `implementation`、設定・データ形式だけを変えるなら `data-config`
- 動作確認だけなら `verification`、確認で見つけた修正まで行うなら `implementation` を主 type にする

例:

- 既存コードを読んで配置方針を出す: `research`
- 設計方針を決める設計書を作る: `design`
- 決定済み設計を README / docs として整える: `documentation`
- 実装してテストも書く: `implementation` + `extra task types: verification`
- README を整備する: `documentation`
- 実装済み内容をレビューする: `review-audit`
- JSON 設定を変更する: `data-config`

## task type / extra task types

### task type

子が主に従う task type です。

- 子は `workspace-plan-executor` の共通ルールと、固定の `references/common-reporting.md` を前提にする
- 子は主 `task type` から対応する `references/task-<type>.md` を読む

### extra task types

主 task type だけでは不足する補助観点です。

例:

- 実装 + 検証: `task type: implementation`、`extra task types: verification`
- 設計 + 監査: `task type: design`、`extra task types: review-audit`
- 文書作成 + ユーザー向け要約: `task type: documentation`、`extra task types: communication-summary`

親は原則として reference パスを手動指定しません。
子が `task type` / `extra task types` から task reference を判断します。

## 委任入力テンプレート

```text
子用 skill `workspace-plan-executor` を使用し、その共通ルールに従ってください。

workspace:
- {WORKSPACE_ROOT}/{backlog|active|archive}/YYYY-MM-DD-作業名/

plan:
- {WORKSPACE_ROOT}/{backlog|active|archive}/YYYY-MM-DD-作業名/plan.md

対象 Todo:
- Tn〜Tm

task type:
- <親が選んだ type>

extra task types:
- （必要時のみ）

plan 抜粋:
- （対象 Todo と必要な前後文脈だけ）

Memory Context:
- （今回の Todo に影響する制約・方針・注意点だけ。なければ「なし」または省略理由）

今回の完了条件:
- 

今回の受入条件:
- 

今回の検証方法:
- 

今回の成果物指定:
- 作成・更新してよいファイル:
- 作成・更新してはいけないファイル:

今回固有の制約:
- 

停止条件:
- 

最終報告:
- 子用 skill の共通報告ルールに従う
- 今回だけ追加で報告してほしい項目があればここに書く
```

## 親が具体的に書くもの

- 何を作れば完了か
- どこまでやれば受入か
- どのファイルを触ってよいか
- どの workspace 成果物へ、何を本文として残してほしいか
- どの確認コマンドを回すか
- どの状況で停止するか
- どの未解決事項を親判断へ戻すか

作成・更新してよい成果物名で迷う場合、親は `references/artifact-candidates.md` を参照して候補を選びます。子には候補一覧全体ではなく、今回許可するファイル名、目的、範囲だけを渡します。

設計、方針、判断、調査、検証、タスク補助情報を正式成果物として残す場合も、本文作成・更新は原則として子へ委任します。親は、成果物候補の選定、委任入力の作成、子への必要情報提供、報告検査、要約統合、必要時の rename / move 判断を担当します。

## 親が毎回書かないもの

- 子が `plan.md` を直接更新しないことの長文説明
- 子が memory を勝手に保存しないことの長文説明
- task type ごとの一般的な注意点
- 報告の共通章立ての詳細

これらは `workspace-plan-executor` 本体と固定 reference に任せます。
