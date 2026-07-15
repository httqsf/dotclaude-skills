# dotclaude-skills

[Claude Code](https://claude.com/claude-code) 用の Agent Skills 集。会社PC・自宅PCなど複数マシンで同じスキルを使えるように公開しています。

現在は、以下の知識スキル群を収録しています。

- **ミノ駆動（仙塲大也）さん**の設計思想をリスペクトした設計スキル群
- **t-wada（和田卓人）さん**が広めてきたTDD・自動テストの知見をリスペクトしたテストスキル群
- **Vlad Khononov『ソフトウェア設計の結合バランス』**の Balanced Coupling モデルをリスペクトした結合設計スキル群

> ⚠️ これらのスキルは、各氏が公開している書籍・登壇資料・記事・インタビューなどを元に、思想や実践を学ぶため非公式にまとめたものです。内容の正確性は原典（各スキル `reference/*.md` の「出典」）を参照してください。各氏本人および所属組織とは関係ありません。

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
├── mino-organization       # 組織への設計文化展開
│   ├── design-learning-workshop
│   └── design-knowledge-scaling
├── t-wada-tdd-core         # Red-Green-Refactor / Canon TDD / 実装戦略
├── t-wada-test-craft       # テストコードの構造・ダブル・テスト容易性
├── t-wada-test-strategy    # テストサイズ・ピラミッド・カバレッジ
├── t-wada-test-reliability # flaky/fragile test・分離・決定性
├── t-wada-quality-mindset  # 品質と速度・レガシーコード・契約
├── t-wada-culture          # テスト文化・学習・AI時代のTDD
├── balanced-coupling-core       # 結合の根幹：知識共有 / 3軸 / BALANCE式
├── balanced-coupling-strength   # 統合強度：侵入/機能/モデル/コントラクト結合
├── balanced-coupling-distance   # 距離：変更の社会技術的コスト・分散モノリス
├── balanced-coupling-volatility # 変動性：変更確率・サブドメイン分類
├── balanced-coupling-review     # 結合レビュー：3軸マッピング→不均衡検出
└── balanced-coupling-rebalance  # 再均衡化：どの軸を動かして是正するか
```

各スキルは `SKILL.md`（司令塔）＋ サブトピックごとの `reference/*.md`（詳細・コード例・出典）で構成しています。

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

配置後、Claude Code を再起動すると `mino-*`・`t-wada-*`・`balanced-coupling-*` のスキルが認識されます。`/` でスキル一覧に表示されれば成功です。

## 使い方

自然言語で設計相談すると、文脈に応じて自動でスキルが起動します。例：
- 「このクラスの命名、ミノ駆動流で見直して」→ `mino-code-design`
- 「このドメインをモデリングしたい」→ `mino-domain-design`
- 「技術的負債、どこから返す？」→ `mino-refactoring`
- 「設計勉強会を企画したい」→ `mino-organization`
- 「TDDでこの機能を実装して」→ `t-wada-tdd-core`
- 「このテスト、モックしすぎていない？」→ `t-wada-test-craft`
- 「テスト戦略とカバレッジ目標を考えたい」→ `t-wada-test-strategy`
- 「flaky testを直したい」→ `t-wada-test-reliability`
- 「テスト文化をチームに根付かせたい」→ `t-wada-culture`
- 「この設計、疎結合にすべき？」→ `balanced-coupling-core`
- 「このコードベースの結合をレビューして」→ `balanced-coupling-review`
- 「分散モノリスになっていない？」→ `balanced-coupling-distance`
- 「崩れた結合バランスをどう直す？」→ `balanced-coupling-rebalance`

同じプレフィックスのスキル群は、相談内容に応じて相互に連携します。

## ライセンス

スキルの記述・構成は [MIT License](./LICENSE)。ただし元となった思想・具体例の著作権は各原著作者に帰属します。学習目的でご利用ください。
