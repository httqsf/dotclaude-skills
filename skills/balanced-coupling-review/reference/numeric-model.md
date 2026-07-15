# numeric-model — 数値モデルの位置づけ（二値判定で足りる）

## 核心

公開ソース（coupling.dev・公式プラグイン）は**意図的に二値（binary）モデルのみ**を提示する。書籍第10章に「Balancing Coupling on a Numeric Scale」節があり、二値から十段階程度の数値スケールへ拡張した方程式が導入されるが、**その具体式はどの公開ソースにも掲載されていない**。そして著者自身のエージェント用公式スキルすら二値XOR則で運用している——レビュー実務には二値判定で足りるというのが著者の設計判断と読める。**式を捏造せず、二値判定で足りる。**

## 判定に使う3式（これが第一線）

```
MODULARITY = STRENGTH XOR DISTANCE          # 互い違いならモジュール性
COMPLEXITY = STRENGTH AND DISTANCE          # 両方高ければ複雑性
BALANCE    = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY   # 低変動なら不均衡でも救済
```

式の導出・含意の詳細は `balanced-coupling-core/reference/balance-formula.md`（正典）。本ファイルはレビューでの運用に絞る。

## 数値スケールについて確認できた事実

- coupling.dev のbalance記事は「簡潔さのため二値モデルで説明する。より精緻な数学的モデルは書籍にある」と明言し、数値・スコアの具体は非掲載
- 公式プラグインも同様に「書籍には連続値のより精緻なモデルがある」と述べた上で、**二値モデルのみで運用**している
- 読者解説・書評では「正確な科学ではないという前置き付きで十値スケールの方程式が導入される」「hypothetical（仮説的）な数値スケール」と紹介されるが、方程式そのものは記事に明示されていない
- したがって、独自に作った式を「Khononovの式」として提示してはならない。数値モデルの中身が必要なら書籍本文を参照するしかない

## 数値を使うとしたら（相対比較のみ）

- **設計案の比較**: 案Aと案Bのペアごとに3軸を評点し、不均衡（強度≈距離が同値×高変動）の数と深刻度を比べる。絶対値でなく相対比較・傾向把握に使う
- **変更前後の比較**: リファクタリング前後で「どの軸をどちらに動かしたか」を明示する。例: 非同期化は距離↑なので、強度↓（契約化）を伴わなければバランスは悪化——「操作＝軸の移動」として記述する
- 閾値・合格点を作らない。「公式スコア」を単独で主張すると疑似科学化する。数値は測定値ではなく、意思決定を構造化する道具

## 実践指針

- 判定の第一線は3式の二値評価。迷う境界例のみ相対比較として粗い数値を使ってよいが、根拠の言語化が本体
- 各軸の評点根拠（何を見てHigh/Lowとしたか）を必ずレビュー文書に残す
- 強度の細分にはコナーセンスを使う（コントラクト/モデル結合内は静的コナーセンス、機能結合内は動的コナーセンスで程度比較。詳細は `balanced-coupling-strength/reference/connascence.md`）
- 「XOR不成立＝即問題」にしない。NOT VOLATILITY での救済を確認してから深刻度を付ける（判定表は `reference/imbalance-patterns.md`）

## 出典

- （一次）https://coupling.dev/posts/core-concepts/balance/（「より精緻な数学的モデルは書籍に」との言及元）
- （一次）https://github.com/vladikk/modularity — `skills/balanced-coupling/SKILL.md`（公式スキルが二値運用である根拠）
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495（第10章の読者解説。方程式は非明示）
- （二次）https://kalabro.tech/balancing-coupling-in-software-design-book/（"hypothetical numeric scale" と紹介する書評）

> 帰属注記: 3式（MODULARITY/COMPLEXITY/BALANCE）は Khononov オリジナル（一次確認済み）。数値スケールは書籍第10章にのみ存在し、具体式は公開ソース未確認——本スキル群では式を提示しない。
