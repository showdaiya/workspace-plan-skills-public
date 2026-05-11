# task type: data-config

## 対象

データ、設定、JSON / YAML / Markdown 表 / CSV / SQL、環境差分、設定値の整理や比較。

## 使う場面

- JSON、YAML、CSV、SQL などのデータや設定を更新・比較するとき
- スキーマ、キー、列、環境差分を確認しながら差分を整理するとき
- downstream 影響や検証方法を含めて、設定変更を報告するとき

## 基本方針

- source of truth を意識する
- source of truth が複数あり衝突する場合は、子で裁定せず stop して親判断待ちにする
- スキーマ、列、キー、制約、フォーマット差分を確認する
- syntax ok と semantic ok は別物として扱い、同じ evidence で混同しない
- 差分と検証方法を明示する
- downstream 影響は confirmed / unconfirmed を分けて返す
- `secrets` や `.env` は読まない・編集しない

## preflight

- source of truth は明確か
- source of truth 同士に競合や版ずれはないか
- 対象ファイルのスキーマや期待構造は明確か
- 構文整合の確認方法と、意味整合の確認方法はあるか
- downstream 影響の確認範囲はどこまでか
- `secrets` 非接触で完了できるか

## 報告で重視すること

- どのデータ / 設定ファイルを触ったか
- 差分の要点
- 検証方法
- syntax ok / semantic ok のどこまで確認したか
- downstream の confirmed / unconfirmed
- 未確認の環境やケース
- secrets に触れていないこと

## continue / stop / partial-complete の例

- continue: source of truth と検証方法が明確で、許可範囲内の更新で閉じる
- stop: source of truth が不明、または複数の source of truth が衝突しており親判断なしに裁定できない
- stop: `.env` や秘密情報に触れないと完了できない
- partial-complete: syntax ok は確認できたが、semantic ok や downstream confirmed は未確認のまま残る

## 境界例

- JSON / YAML / SQL の差分更新と構文確認: data-config
- syntax ok をそのまま semantic ok と断定する: 不可
- downstream 影響が未確認なら、confirmed / unconfirmed を分けて partial または親判断待ちとして返す

## 停止すべき場面

- source of truth が不明、または source of truth 間で競合している
- `.env` や秘密情報に触れないと完了できない
- データ破壊や大規模 migration が必要だが許可がない
