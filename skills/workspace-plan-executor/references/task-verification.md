# task type: verification

## 対象

テスト、検証、再現確認、品質確認、UI 検証、Playwright を含む汎用的な確認作業。

## 使う場面

- 実装済み変更や既存挙動のテスト・検証を行うとき
- バグ再現確認や受入条件の確認結果を返すとき
- screenshot、log、report を含む確認結果を整理して報告するとき

## 基本方針

- 何をどう確認したかを記録する
- 成否判定は受入条件ごとに `pass / fail / partial / not-run / 判定不能` を使い分け、推測で埋めない
- 未検証範囲を隠さない
- diagnostics は親が許可した範囲に留め、観測・切り分け・再実行条件整理までに留める
- 修正可否は verification と別に確認する
- 再現確認では、再現した / しない / 判定不能を区別する
- blocked / partial では safe return を優先し、未実施や判定不能を明示して返す
- stop trigger で止まる場合は、共通 packet に `不足情報` / `再開条件` / `未検証範囲` を分けて返す
- `extra task types` があっても、それを根拠に許可範囲を広げない

## preflight

- 検証対象と pass / fail 基準は明確か
- retry policy や再現回数の期待があるか
- fixture / seed / 前提データが必要か
- 失敗時に fix してよいか、diagnostics だけか
- 受入条件ごとの結果分類で、`not-run` や `判定不能` を返してよい境界が明確か
- `evidence` や `temp script` の許可範囲が明確か

## 記録すること

- 実行したコマンドやシナリオ
- 確認した環境や前提
- 成功条件との対応
- 失敗時の症状やログ
- diagnostics をどこまで行い、どこで止めたか
- 未実施・未検証の範囲

## 結果分類の例

- `pass`: 受入条件または対象シナリオを満たしたと判断できるだけの evidence がある
- `fail`: 条件を満たさず、失敗内容をその受入条件に対応づけて確認できた
- `partial`: 一部だけ実行・確認できた。確認済み範囲と未確認範囲を分けて返す
- `not-run`: 前提不足や時間制約で未実行。過去ログや推測で pass / fail 扱いしない
- `判定不能`: 実行はしたが evidence 不足、結果の衝突、ノイズで pass / fail を確定できない
- `flaky` / `infra fail` / `app fail`: 必要なら補助ラベルとして付ける

`blocked` は受入条件マトリクスの結果分類ではなく、共通 packet 側の結果サマリや状態判断材料で返す。各受入条件には `partial` / `not-run` / `判定不能` を割り当てて理由を残す。

## UI / Playwright を含む場合

- シナリオ名や操作手順を短く残す
- 生成した screenshot / log / report のパスを返す
- 自動化できなかった確認は手動確認項目として分ける

## 報告で重視すること

- 何が確認済みか
- 何が未確認か
- 再実行しやすい情報があるか

## continue / stop / partial-complete の例

- continue: シナリオ、前提、pass / fail 基準が揃っている
- stop: fixture / seed や実行環境がなく、前提を満たせない。対象シナリオは `not-run` で返し、必要な前提を明記する
- stop: 範囲外の実装修正や許可外 diagnostics がないと先へ進めない。修正せず、観測結果と再開条件だけ返す
- stop: `extra task types` に implementation が含まれていても、verification の許可範囲を広げる根拠にはしない。必要な修正は不足情報、再開条件、未検証範囲として返す
- partial-complete: smoke は `pass` だが、seed 必須シナリオはデータ不足で `not-run`
- partial-complete: 再現はしたがログが不足し、原因切り分けは `判定不能` のまま返す

## 境界例

- screenshot、log、report を残す: verification の `evidence`
- 失敗原因の最小切り分けや再実行条件の整理まで行う: verification
- 失敗後にアプリやテストを修正する: implementation の明示許可が必要
- 補助スクリプトで再現確認する: 親の明示許可がある場合だけ
