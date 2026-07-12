# dotclaude-skills

[Claude Code](https://claude.com/claude-code) 用の Agent Skills 集。会社PC・自宅PCなど複数マシンで同じスキルを使えるように公開しています。

現在は **ミノ駆動（仙塲大也）さんの設計思想をリスペクトした設計スキル群** を収録しています。

> ⚠️ これらのスキルは、ミノ駆動さんが公開している登壇資料・記事（Speaker Deck / Qiita / 各種インタビュー）を元に、設計思想を学習・実践するために非公式にまとめたものです。内容の正確性は原典（各スキル `reference/*.md` の「出典」）を参照してください。ミノ駆動さん本人および所属組織とは関係ありません。

## 収録スキル

```
skills/
├── mino-design-core        # 設計の根幹：目的→課題→手段 / 品質特性 / 文脈解釈
│   ├── purpose-goal-means
│   ├── quality-attributes
│   └── context-interpretation
├── mino-domain-design      # ドメインモデリング
│   ├── bounded-context-discovery
│   ├── ubiquitous-language
│   ├── invisible-driven-modeling
│   ├── invariant-modeling
│   └── data-destruction-analysis
├── mino-code-design        # クラス/メソッド設計
│   ├── purpose-centered-encapsulation
│   ├── interface-branch-reduction
│   ├── purpose-driven-naming
│   └── purpose-driven-abstraction
├── mino-refactoring        # リファクタリング / 技術的負債
│   ├── debt-prioritization
│   ├── legacy-purpose-split
│   └── ai-assisted-refactoring
└── mino-organization       # 組織への設計文化展開
    ├── design-learning-workshop
    └── design-knowledge-scaling
```

各スキルは `SKILL.md`（司令塔）＋ サブトピックごとの `reference/*.md`（詳細・コード例・出典）で構成。全体を「**目的 → 課題 → 手段**」の連鎖で貫いています。

## インストール

Claude Code はユーザースキルを `~/.claude/skills/<skill-name>/SKILL.md` から読み込みます。以下のいずれかで配置してください。

### 方法A: シンボリックリンク（推奨・`git pull` で更新反映）

```bash
git clone git@github.com:httqsf/dotclaude-skills.git ~/dotclaude-skills
mkdir -p ~/.claude/skills
for d in ~/dotclaude-skills/skills/*/; do
  ln -sfn "$d" ~/.claude/skills/"$(basename "$d")"
done
```

更新するときは `cd ~/dotclaude-skills && git pull` だけ。

### 方法B: コピー（シンプル）

```bash
git clone https://github.com/httqsf/dotclaude-skills.git
mkdir -p ~/.claude/skills
cp -R dotclaude-skills/skills/* ~/.claude/skills/
```

### Windows（会社PCがWindowsの場合・PowerShell）

```powershell
git clone https://github.com/httqsf/dotclaude-skills.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse -Force dotclaude-skills\skills\* "$HOME\.claude\skills\"
```

配置後、Claude Code を再起動すると `mino-*` スキルが認識されます。`/` でスキル一覧に表示されれば成功です。

## 使い方

自然言語で設計相談すると、文脈に応じて自動でスキルが起動します。例：
- 「このクラスの命名、ミノ駆動流で見直して」→ `mino-code-design`
- 「このドメインをモデリングしたい」→ `mino-domain-design`
- 「技術的負債、どこから返す？」→ `mino-refactoring`
- 「設計勉強会を企画したい」→ `mino-organization`

いずれも `mino-design-core`（目的思考）を前提に連携します。

## ライセンス

スキルの記述・構成は [MIT License](./LICENSE)。ただし元となった設計思想・具体例の著作権はミノ駆動さんに帰属します。学習目的でご利用ください。
