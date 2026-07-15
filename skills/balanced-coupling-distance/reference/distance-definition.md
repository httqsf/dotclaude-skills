# distance-definition — 距離の定義（共進化の社会技術的コスト）

## 核心

距離とは、結合した2つのコンポーネントを共進化（co-evolve）させるときに発生する**社会技術的コスト**である。著者の定義逐語: "the socio-technical cost of co-evolving components (code structure, team boundaries, runtime dependencies)"——コード構造・チーム境界・ランタイム依存の3要因を明示的に含む。統合強度が「連鎖変更が起きる確率」を表すのに対し、距離は「連鎖変更が起きたときのコスト」を表す（"Distance determines the cost of a cascading change"）。

## 実践指針

- 距離を「ディレクトリがどれだけ離れているか」だけで判断しない。①コード構造 ②組織構造（チーム境界）③ランタイム依存の3要因で評価する。
- コストの正体は**認知負荷と調整コスト**。"The physical distance between the source code of coupled components affects the cognitive effort required to change them simultaneously."（coupling.dev）。同一ファイル内の2オブジェクトの共進化は、別マイクロサービス・別システムの2オブジェクトより圧倒的に安い。
- 距離には**逆向きの力＝ライフサイクル結合**がある。"The closer the components are, the stronger their lifecycle interdependency: they will need to be tested and deployed together."（coupling.dev）。近づけると共進化は安くなるが、テスト・デプロイの運命共同体になる。距離は一方向に最小化すべき量ではなく、トレードオフとして設計する。
- ランタイム結合も距離を変える。同期統合はライフサイクル結合を強め、**非同期化は実効距離を広げる**。著者は「非同期メッセージングで距離を広げたら、その分だけ統合強度を下げて補償せよ」と述べる（vladikk/modularity balanced-coupling/SKILL.md の趣旨を再構成）。
- 距離は**観察している抽象レベルに相対的**。"Distance is not an absolute measure — it is relative to the level of abstraction being analyzed. At any given level of abstraction, the highest distance is the boundary of that level."——単一デプロイ・1人開発でも、モジュールレベルで見ればモジュール間境界がそのレベルの最大距離であり、高距離結合の問題は起こりうる。

## 具体例のエッセンス

- 本人発言（InfoQ）: "If you have two objects in the same file, then the distance between the source code of the two objects is short. However, if those two objects belong to different microservices, then you have different code bases, different projects, different repositories, maybe even different teams."
- 距離は「知識が移動する空間」。本人発言: "the longer the distance that is traveled by the knowledge, the sooner it'll cause that cognitive overload."
- DDD Europe 2020: "The longer the distance, the more effort must be invested in order to coordinate a change that affect both components."
- 3軸での位置づけと総合判定（`BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY`）の詳細は `balanced-coupling-core/reference/balance-formula.md` を参照。本ファイルは距離軸単体の定義のみを扱う。

## 出典

- （一次）https://github.com/vladikk/modularity （README定義・skills/balanced-coupling/SKILL.md §2.2 Distance。CC BY-NC-SA 4.0のため手順・判定基準は再構成）
- （一次）https://coupling.dev/posts/dimensions-of-coupling/distance/
- （一次・本人発言）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://www.infoq.com/news/2020/02/balancing-coupling-ddd-europe/ ※DDD Europe 2020講演レポート

> 帰属注記: 「距離」を強度・変動性と並ぶ結合の一次元として定式化したのはKhononovオリジナル。定義逐語・引用英文はすべて著者一次ソース（プラグインREADME/SKILL.md・coupling.dev・InfoQ本人発言）で確認済み。書籍本文は未参照のため、書籍の逐語としては引用しない。
