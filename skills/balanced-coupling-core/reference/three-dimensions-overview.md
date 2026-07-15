# three-dimensions-overview — 3軸の概要と相互関係

## 核心

結合の善し悪しはコンポーネント自身ではなく「接続の設計」で決まり、3つの次元で評価する。本人による一文定義: "Integration strength reflects the **likelihood** of a change propagating across boundaries. Distance determines the **cost** of a cascading change. Volatility reflects the **probability of a component needing to change in the first place**."——強度が「起きやすさ」、距離が「痛さ」、変動性が「そもそも起きるか」を担う役割分担。

本ファイルは概要のみ。各軸の深掘りは軸別ファミリーが正典（下記委譲先参照）。3軸を束ねる判定式は `balanced-coupling-core/reference/balance-formula.md` が正典。

## 各軸の一言サマリ

**統合強度（Integration Strength）** — 共有される知識の量と性質→連鎖変更の確率。知識を定量測定せず「種類で分類」する4段階: 侵入結合（最強）→機能結合→モデル結合→コントラクト結合（最弱）。強いほど暗黙的で壊れやすい。
→ 詳細は `balanced-coupling-strength/reference/integration-strength-four-levels.md`

**距離（Distance）** — 連鎖変更のコスト。メソッド→オブジェクト→名前空間→サービス→システムと抽象化レベルを上がるごとにコスト増。距離は社会技術的（socio-technical）: 同一チームの2サービスは別チームの2サービスより実効距離が近い（Conwayの法則）。近い=共進化が安いがライフサイクル密、遠い=独立ライフサイクルだが共進化が高価、というトレードオフ。
→ 詳細は `balanced-coupling-distance/reference/distance-spectrum.md`

**変動性（Volatility）** — そもそも変更が発生する確率。評価はcommit履歴ではなくビジネスドメイン視点で: DDDのサブドメイン分類（コア=最高変動／支援=低変動／汎用=実装変動は可変）を第一の道具にする。本質的変動性と偶発的変動性を区別する。
→ 詳細は `balanced-coupling-volatility/reference/domain-classification.md` / `volatility-definition.md`

## 相互関係

- "The longer the distance that is traveled by the knowledge, the sooner it'll cause that cognitive overload."（InfoQ）——知識が移動する距離が長いほど認知過負荷が早く来る（強度×距離の掛け算的関係）
- 高強度×高距離でも変動性が低ければ実害なし。「痛み」は3軸の重なりとして現れる

## 実践指針

- 依存関係を見たら必ず3つの質問をセットで: 「何の知識を共有？（強度）」「一緒に変えるのにいくらかかる？（距離）」「そもそもどれくらい変わる？（変動性）」
- 1軸だけの評価（依存本数カウント等）を避ける。1次元評価は全体像を見誤る
- 距離の評価にはコード構造・チーム構造・ランタイム依存（同期/非同期）の3要素を含める
- 変動性はgit logでなく事業戦略から評価する
- 非同期化・サービス分割など「距離を動かす」施策は、必ず強度側の補償（契約化）とセットで行う
- 3軸の総合判定は `balanced-coupling-core/reference/balance-formula.md`、レビュー手順は `balanced-coupling-review` へ

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §2
- （一次）https://coupling.dev/posts/dimensions-of-coupling/integration-strength/ ほかdimensions配下
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://blog.sa2taka.com/post/coupling-history-and-balanced-coupling/ ※歴史との接続

> 帰属注記: 3軸統合（強度×距離×変動性）はKhononovのオリジナル貢献——従来は暗黙だった距離・変動性を明示軸に昇格させた。古典的モジュール結合度はStevens/Myers/Constantine (1974) 由来、コナーセンスはMeilir Page-Jones (1992) 由来、サブドメイン分類はEric Evans / DDDコミュニティ由来、Conwayの法則はMel Conway由来。
