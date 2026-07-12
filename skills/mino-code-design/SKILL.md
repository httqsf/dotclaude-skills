---
name: mino-code-design
description: ミノ駆動（仙塲大也）流のクラス・メソッドレベルのコード設計を適用する。目的中心のカプセル化（完全コンストラクタ・値オブジェクト・ファーストクラスコレクション・getter/setter回避）、条件分岐の削減（early return・interface・ストラテジー/ポリシーパターン）、目的駆動名前設計、目的駆動の抽象化を扱う。クラスやメソッドを実装するとき、命名に迷うとき、if/switchのネストや重複を減らしたいとき、コードレビューで設計品質を見るときに起動。前提として mino-design-core（目的→課題→手段）を踏まえる。
---

# mino-code-design — ミノ駆動流コード設計

クラス・メソッドレベルで**変更容易性**と**目的中心**を実現する具体テクニック。ドメインモデルの粒度は `mino-domain-design`、既存コードの改善は `mino-refactoring` へ。

> 前提：`mino-design-core` の「目的→課題→手段」。4トピックすべての共通母体は「**目的（purpose）駆動**」と「**関心の分離／疎結合高凝集**」。

## いつ・どう使うか

コードを書く／レビューするとき、次を順に当てる:

1. **命名** → 存在ベース（商品・ユーザー・Manager）でなく目的ベース（注文品・請求金額）で名付けたか（`purpose-driven-naming`）
2. **カプセル化** → データとロジックが同じクラスに凝集し、不正状態を作れないか（`purpose-centered-encapsulation`）
3. **分岐** → if/switchのネスト・重複を early return / interface で消せないか（`interface-branch-reduction`）
4. **抽象化** → 分類（is-a）でなく目的（何のため）で抽象の軸を選んだか（`purpose-driven-abstraction`）

## サブトピック（詳細は reference/ 参照）

| トピック | 一言 | 詳細 |
|---|---|---|
| 目的中心カプセル化 | 完全コンストラクタ・値オブジェクト・不変・getter/setter回避 | `reference/purpose-centered-encapsulation.md` |
| 分岐削減 | early return・interface・ストラテジー/ポリシーで分岐消去 | `reference/interface-branch-reduction.md` |
| 目的駆動名前設計 ★中核 | 存在でなく目的で命名。技術駆動命名を禁止 | `reference/purpose-driven-naming.md` |
| 目的駆動の抽象化 | 分類でなく目的で抽象化の軸を選ぶ | `reference/purpose-driven-abstraction.md` |

## クイック原則（本人語彙）
- 「クラス設計とは**インスタンス変数を不正状態に陥らせないためのしくみづくり**」
- 「**強く関係するものはカプセル化、関係が弱いものは関心の分離**」
- 「**特化こそ正義、半端な汎用化は開発者の心身を滅ぼす**」
- 「ネストの深淵を見つめる時、深淵もまたお前を見つめている」（＝深いネストを避けよ）
- 「**目的不明オブジェクト（UPO: Unknown Purpose Object）**」は要リファクタリング

## アンチパターン検知（赤信号）
- `〜情報`／`〜データ` という名前（ドメインモデル貧血症）
- `Int〜`／`〜Flag`／`tmp`／`data01` 等の技術駆動命名・連番命名
- getter/setterで生データを露出（振る舞いでなく状態を外に晒す）
- 同じ種別で分岐する switch が複数箇所に散在
- 「仮の」「確定した」を形容詞で状態管理（別クラスに分けるサイン）
