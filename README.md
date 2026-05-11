# workspace-plan skills public

OpenCode の workspace / plan 運用向け skill と、対応する workspace テンプレートをまとめた公開用リポジトリです。

## 含まれるもの

- `skills/workspace-plan-owner/`
  - 親エージェント向けの owner / controller skill
- `skills/workspace-plan-executor/`
  - 子エージェント向けの executor / reporter skill
- `workspaces/_template/`
  - `plan.md` と `requirements.md` のテンプレート
  - workspace テンプレート置き場の README

## ディレクトリ構成

```text
skills/
  workspace-plan-owner/
  workspace-plan-executor/
workspaces/
  _template/
    README.md
    plan-template.md
    requirements-template.md
```

## 使い方

このリポジトリの template は、そのまま `/agent_work/workspaces/_template/` へ配置して使う想定です。

例:

```text
/agent_work/.opencode/skills/workspace-plan-owner/
/agent_work/.opencode/skills/workspace-plan-executor/
/agent_work/workspaces/_template/README.md
/agent_work/workspaces/_template/plan-template.md
/agent_work/workspaces/_template/requirements-template.md
```

`workspace-plan-owner` は新しい workspace と `plan.md` / `requirements.md` を作成・更新する owner 側の運用を扱います。
`workspace-plan-executor` は owner から委任された Todo 範囲だけを実行し、結果を報告する executor 側の運用を扱います。

## 補足

この repository は、OpenCode 上で使っている運用を外部共有しやすい形に切り出したものです。
環境依存のパスや運用ルールがあるため、利用時は自分の環境に合わせて調整してください。
