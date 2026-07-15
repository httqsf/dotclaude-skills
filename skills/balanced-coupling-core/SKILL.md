---
name: balanced-coupling-core
description: Vlad Khononov『ソフトウェア設計の結合バランス』(Balancing Coupling in Software Design)のBalanced Couplingモデルの根幹を適用する。結合＝知識共有、複雑性とモジュール性、3軸（統合強度・距離・変動性）の概要、BALANCE式（疎結合と高凝集は同じ式の2形態）、フラクタル性を扱う。設計判断の理由を説明するとき、結合の良し悪しを判断するとき、「疎結合にすべきか」迷うとき、モジュール境界・サービス分割を考えるとき、密結合/低凝集の指摘に反論・裏付けが要るとき、他のbalanced-coupling-*スキル（strength/distance/volatility/review/rebalance）の前提として必ず起動。
---

# balanced-coupling-core — Balanced Couplingモデルの根幹

Vlad Khononov（vladikk）のBalanced Couplingモデルの土台。他のbalanced-coupling-*スキルはすべてこの前提の上に立つ。結合は排除すべき悪ではなく「システムの価値を部分の総和より大きくするもの」——問題は結合するか否かではなく、**どう結合するか**。

> 結合の3軸 — **統合強度**: どの種類の知識を共有しているか（連鎖変更の確率）／**距離**: 一緒に変更する社会技術的コスト（連鎖変更のコスト）／**変動性**: そもそも変更が起きる確率。
> 均衡則: `BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY` — 強い結合は近くに置く。遠くに置くなら知識共有を減らす。低変動なら不均衡でも実害は小さい。

## いつ・どう使うか（診断テーブル）

| 場面・症状 | 当てる概念 | 詳細 |
|---|---|---|
| 「この依存は良いのか悪いのか」 | 依存の本数でなく共有知識の量と性質を見る | `reference/coupling-as-knowledge.md` |
| 変更の影響が事前に予測できない | 複雑性＝因果が事後にしかわからない状態 | `reference/coupling-and-complexity.md` |
| フォルダ分割したのに変更が楽にならない | モジュール＝知識を隠蔽する境界（2基準で点検） | `reference/modularity-and-abstraction.md` |
| 依存を評価する視点が定まらない | 3つの質問（何を共有？いくらかかる？変わる？） | `reference/three-dimensions-overview.md` |
| 疎結合派と高凝集派で議論が割れる | 両者は同じBALANCE式の2解 | `reference/balance-formula.md` |
| クラスの話とサービスの話が混線する | フラクタル性——先に抽象化レベルを宣言 | `reference/fractal-geometry.md` |

## 行動原則

- 「疎結合にせよ」から始めない。まず「この依存で**何の知識**が共有されているか」を問う
- 結合ゼロを目標にしない。完全独立のコンポーネント群はもはやシステムではない
- 依存を見たら3質問をセットで: 何を共有？（強度）／一緒に変えるといくら？（距離）／そもそも変わる？（変動性）
- 判定はBALANCE式で言語化する: 強い結合は近くに、遠くに置くなら契約まで知識を削る
- モデル適用前に必ず観察している抽象化レベルを宣言する（レベル次第で同じ結合が均衡にも不均衡にも見える）
- 暗黙の知識共有（重複実装された業務ルール・他者のDB直読み）を最も警戒する

## 適用範囲（過剰適用を避ける）

- 既存コードベースの規約・フレームワーク慣習を優先。衝突時は規約に従い、改善は提案に留める
- 小規模タスク・使い捨てスクリプトに境界設計の重装備を持ち込まない
- 低変動な安定依存を「不均衡だから」と機械的に直さない——低変動性は不均衡を中和する（式のとおり）

## 姉妹スキル

軸の深掘りは `balanced-coupling-strength` / `balanced-coupling-distance` / `balanced-coupling-volatility`、不均衡の検出は `balanced-coupling-review`、是正は `balanced-coupling-rebalance` が担当。
