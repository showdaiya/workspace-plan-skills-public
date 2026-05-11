# task type: refactor-cleanup

## 対象

リファクタリング、整理、削除、rename / move、不要物除去、構造の軽い整理。

## 使う場面

- 構造の軽い整理や責務の見直しを行うとき
- 明示許可のある delete / rename / move を伴う整理をするとき
- 不要物除去や影響範囲確認を含む cleanup を進めるとき

## 基本方針

- 変更範囲を限定する
- delete / rename / move は対象と理由が明示許可された場合だけ行う
- behavior-preserving を既定とし、変わる可能性があるなら明示または停止する
- ロールバック観点を持って変更し、不整合な中間状態を残さない
- mechanically required な最小追随差分と、ついでの cleanup を分ける
- 振る舞い変更があるなら明示する

## preflight

- 何を整理対象とするか明確か
- 許可された変更種別は何か
- cleanup が連鎖しうる変更範囲はどこまでか
- 依存先や参照先への影響を確認したか
- ロールバック時に戻すべき箇所を把握しているか
- delete / rename / move が個別に許可されているか、単なる cleanup 許可を拡大解釈していないか
- mechanically required な追随差分と、見送り可能な追加 cleanup を分けられているか

## 報告で重視すること

- 変更した範囲
- 削除・移動・rename の有無
- 削除したもの / 削除しなかったもの
- 影響範囲
- behavior-preserving と判断した根拠、または崩れる恐れ
- rollback で戻す単位と、未実施にした cleanup 案
- 確認したこと / 未確認のこと
- 追加で親判断が必要な整理案

## continue / stop / partial-complete の例

- continue: 許可済み cleanup が限定範囲で閉じ、影響範囲も追える
- continue: delete / rename / move が明示許可済みで、追随差分も mechanically required な範囲に留まる
- stop: 明示許可のない削除・移動が必要
- stop: rename / move の影響が広がり、behavior-preserving や rollback の見通しが崩れた
- stop: 影響範囲が想定より広く、構造再編に広がる
- partial-complete: 許可済み cleanup だけ実施し、追加 cleanup 候補は親判断待ちで返す
- partial-complete: mechanically required な追随差分までは実施したが、「ついで cleanup」は見送って返す

## 境界例

- import 整理や整形など mechanically required な最小追随差分: 許可範囲内なら可
- delete / rename / move に伴う参照更新や import 修正など、成立に必須な追随差分: 許可範囲内なら可
- 「ついでに」別箇所まで整理する: 範囲外
- rename / move を検討するだけで実行しない: 変更案として報告
- 軽い cleanup のつもりで振る舞い変更や実装追加を混ぜる: implementation 側の責務であり範囲外

## 停止すべき場面

- 明示許可のない delete / rename / move が必要
- 影響範囲が想定より広い
- behavior-preserving と言い切れないのに、検証や rollback 観点も不足している
- 安全に戻せる見通しが立たない
