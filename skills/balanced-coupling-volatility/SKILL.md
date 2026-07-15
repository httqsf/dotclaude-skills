---
name: balanced-coupling-volatility
description: Vlad Khononov『Balancing Coupling in Software Design』（邦訳『ソフトウェア設計の結合バランス』）流の第3軸「変動性＝変更がそもそも起きる確率」の評価を適用する。変動性の定義（本質的/偶発的変動性・偶発的非変動性）、DDDサブドメイン分類（core/supporting/generic）による変動性評価、推定手段（ビジネス戦略・Wardley Maps、NOT commit history）、バランス式での中和効果、共有コードの判定を扱う。コンポーネントの変更頻度・将来の変わりやすさを見積もるとき、共通ライブラリに何を置くか決めるとき、コア/サポート/汎用サブドメインを分類するとき、「安定しているから大丈夫」「レガシーだから触らない」と判断する前、git履歴から変動性を語りたくなったときに起動。3軸の総合判定は balanced-coupling-core、不均衡の検出手順は balanced-coupling-review が担当。
---

# balanced-coupling-volatility — 変動性（変更が起きる確率）

Khononovの結合3軸のうち「時間の次元」を担うファミリー。統合強度が「変更が伝播する可能性」、距離が「連鎖変更のコスト」なら、変動性は「その変更がそもそも起きるのか」。Parnas（1972・情報隠蔽）の「変わりやすい判断を隠せ」に対し、「何が変わりやすいかをどう見積もるか」を体系化したのがKhononovの貢献。

> 結合の3軸 — **統合強度**: どの種類の知識を共有しているか（連鎖変更の確率）／**距離**: 一緒に変更する社会技術的コスト（連鎖変更のコスト）／**変動性**: そもそも変更が起きる確率。
> 均衡則: `BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY` — 強い結合は近くに置く。遠くに置くなら知識共有を減らす。低変動なら不均衡でも実害は小さい。

## いつ・どう使うか（診断）

| 状況 | 最初の問い | 対処 |
|---|---|---|
| 変わりやすさを見積もりたい | この領域は競争優位を生むか？（コア＝最高変動） | `reference/domain-classification.md` → `reference/estimating-volatility.md` |
| git logで変動性を測ろうとしている | その頻度は本質的か偶発的（設計不良）か | `reference/estimating-volatility.md` |
| 「安定しているから大丈夫」と言いたい | 怖くて触れないだけでは？（偶発的非変動性） | `reference/volatility-definition.md` |
| 不均衡（強×遠）を見つけたが直すべきか迷う | 変動性は低いか？　低なら tolerable technical debt | `reference/volatility-in-balance.md` |
| 共通ライブラリに何を置くか迷う | その知識は低変動×汎用か | `reference/volatility-and-shared-code.md` |

## サブトピック（詳細は reference/ 参照）

| トピック | 一言 | 詳細 |
|---|---|---|
| 変動性の定義 | Parnas系譜。本質的/偶発的変動性、偶発的非変動性 | `reference/volatility-definition.md` |
| サブドメイン分類 | core/supporting/generic の**正典**。DDDサブドメインが変動性評価の第一の道具 | `reference/domain-classification.md` |
| 変動性の推定 | ビジネス視点で見積もる。NOT commit history | `reference/estimating-volatility.md` |
| バランス式での役割 | `NOT VOLATILITY`＝実利の項。低変動は不均衡を中和する | `reference/volatility-in-balance.md` |
| 共有コード | 共有してよいのは低変動×汎用の知識のみ | `reference/volatility-and-shared-code.md` |

## 行動原則

- 変動性は**ビジネスドメインから**見積もる。git logやコード構造からではない
- 高頻度変更を見たら「本質的（ドメインが変わる）か偶発的（設計が悪い）か」を必ず仕分ける
- 静かなコンポーネントには「本当に安定か、触るのが怖くて放置されているだけか」を問う
- コアサブドメイン＝最高変動。統合強度×距離のXORバランスへの設計投資をそこに集中する
- 低変動を理由に不均衡を許容したら、根拠と再評価トリガーをセットで記録する（許容≠良い設計）
- 数値モデルは書籍第10章にあるが公開ソースで式は未確認。二値XOR則で判断せよ（著者の公式スキルも二値運用）

## 適用範囲（過剰適用を避ける）

- 既存コードベースの規約・フレームワーク慣習を優先。衝突時は規約に従い、改善は提案に留める
- 低変動な安定依存を「不均衡だから」と機械的に直さない。強×遠×低変動は記録して劣後でよい
- 変動性の見積もりは予言ではなくヒューリスティック。将来要件の先回り実装の口実にしない
- 小規模タスク・スクリプトにサブドメイン分類の重装備を持ち込まない

## アンチパターン検知（赤信号）

- コミット数・変更行数だけで「高変動」「安定」とラベリングしている
- 共有ライブラリに User/Order/Payment などのドメインモデルが入っている
- 1つの機能追加が常に「共有ライブラリ＋複数サービス」の同時変更になっている
- ビジネス上重要な領域が「レガシーだから触らない」で長期間放置されている（機会損失のシグナル）
