# 計画タイトル

このテンプレートは、各 workspace 内の `plan.md` として保存します。

workspace root は `{WORKSPACE_ROOT}` です。workspace の状態は `backlog` / `active` / `archive` の親ディレクトリで表し、plan 本体名は常に `plan.md` とします。

## 基本情報
- 計画ID:
- 作成日: YYYY-MM-DD
- 更新日: YYYY-MM-DD
- 作成者: 親エージェント
- 対象:
- 最終的な目的:
- 成功条件:
- 成果物:
  - `plan.md`: 進行管理
  - `requirements.md`: ユーザー要件の初期正本
- 前提条件:
- 対象外:
- 開始条件:
- 停止条件:
- 関係ファイル/場所:
- 要件ファイル: requirements.md
- workspace root: {WORKSPACE_ROOT}
- ワークスペース: {WORKSPACE_ROOT}/{backlog|active|archive}/YYYY-MM-DD-作業名/
- 参照元 archive workspace: 必要時のみ記載

## 全体の禁止事項
- plan の大枠を崩す変更を、ユーザー確認なしで行わない
- plan の更新は親エージェントのみが行う
- 子エージェントに `plan.md` を直接更新させない
- 対象外に書かれたことを、この plan に混ぜない
- workspace の状態を plan ファイル名で管理しない
- workspace 内の plan 本体名を `plan.md` 以外にしない
- ユーザーの明示指示なしに memory topic を更新しない
- 対象外に書いた実体作成、外部保存、実 skill 作成を勝手に行わない
- 親が明示していない削除・リネーム・移動を子に実行させない
- archive workspace は原則読み取り専用として扱う

## 全体の補足
- 判断メモ:
- 未解決事項:

## 主要成果物一覧
| path | 種別 | 関連Todo | 状態 | 用途 |
| --- | --- | --- | --- | --- |
| `plan.md` | plan | 全体 | active | 進行管理 |
| `requirements.md` | requirements | 全体 | active | ユーザー要件の初期正本 |

## 引き継ぎと注意事項
- 注意点:
- 引き継ぎ内容:
- 完了後の扱い: 完了・cancelled などで終了したら workspace ごと `{WORKSPACE_ROOT}/archive/` へ移す。再開時は archive を直接戻さず、新しい workspace を作り参照元を明記する。
- blocked / 待機時の扱い: blocked ディレクトリは作らない。短期 blocked は active に残し、長期待ちは親判断で backlog、終了扱いは archive に移す。
- 環境特有のこと:

## 第1章 章タイトル
- この章の目的:

### Todo一覧
- T1 Todo 内容
  状態: pending
  完了条件:
  参照範囲:
  メモ:

- T2 Todo 内容
  状態: pending
  完了条件:
  参照範囲:
  メモ:

## 第2章 章タイトル
- この章の目的:

### Todo一覧
- T3 Todo 内容
  状態: pending
  完了条件:
  参照範囲:
  メモ:

- T4 Todo 内容
  状態: blocked
  完了条件:
  参照範囲:
  理由:
  不足情報:
  再開条件:
  メモ:

- T5 Todo 内容
  状態: cancelled
  完了条件:
  参照範囲:
  理由:
  メモ:

## 委任メモ欄

子へ委任する場合、親は短い委任入力を作る。

親入力に含める主なもの:

- 子用 skill を使用し、その共通ルールに従うこと
- workspace パス
- plan パス
- 対象 Todo
- plan 抜粋
- 今回の完了条件
- 今回の成果物指定
- 今回固有の制約
- 最終報告に含めること

必要に応じて短く追加する任意ブロック:

- draft skill 期間の参照先
- 実装作業用: 作業対象ファイル、workspace 成果物、テスト・確認コマンド
- 調査作業用: 外部調査可否、優先情報源、出典記録、調査深度
- blocked 想定: 理由、不足情報、再開条件、判断依頼、実施済み/未完了、作成・更新済み、未検証、次アクション
- archive 参照: 読み取り専用、新規 workspace 継続
- 補助成果物追加: 明示許可、軽微な補助成果物、主要成果物 / 構造変更
- 削除・リネーム・移動可否
- 並行委任時の同一ファイル同時編集禁止

親入力に毎回長く入れないもの:

- 原本参照ルールの詳細
- memory / plan / workspace 境界の長文
- workspace 状態説明全文
- 子用 skill の checklist 全文

## 実行ログ
- YYYY-MM-DD 親エージェントが plan 初版と requirements 初版を作成
- YYYY-MM-DD 親エージェントが T1〜T2 を短い委任入力で子エージェントへ委任
- YYYY-MM-DD 子エージェントが子用 skill の参照判断に従い、必要最小限で `plan.md` 原本を参照
- YYYY-MM-DD 子エージェントが参照範囲拡張に留め、作業範囲拡張は行わず報告
- YYYY-MM-DD 親エージェントが子の報告を受けて `plan.md` を更新
