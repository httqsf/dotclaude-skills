# review-checklist — レビュー用チェックリスト（4段構成）

## 核心

チェックは「A. 統合強度 → B. 距離 → C. 変動性 → D. バランス判定」の4段で回す。1軸だけの評価は禁じ手（公式制約: 結合を単一次元だけで評価しない）。各軸の詳細定義は正典ファミリーへ委譲し、本ファイルはレビュー時の点検手順に絞る。

## A. 統合強度チェック（強い順に疑う）

```
□ 侵入結合: 相手のprivateインターフェース・実装詳細で統合していないか
   （内部DB直接参照／privateオブジェクト／非公開API → 全実装知識を共有と見なす）
□ 機能結合: 相手の機能要件・ビジネスルール・処理順序の知識を共有していないか
   （極端形＝ビジネスルールの重複実装。実行順序・タイミング・補償処理の共有）
□ モデル結合: 同じビジネスドメインモデルを共有していないか
   （モデルが変われば全共有者が変わる）
□ コントラクト結合: 統合専用の契約のみの共有か（最弱・最も明示的。
   Facade／Open-host service・公開言語／ACL／DTO）
□ 共有知識は暗黙的か明示的か（暗黙＝重複ルール・DB直アクセス・内部挙動への仮定は特に危険）
□ 細分が必要なら: 静的コナーセンスでcontract/model内を、動的コナーセンスでfunctional内を比較
```

4段階の詳細・コードからの検出法は `balanced-coupling-strength/reference/integration-strength-four-levels.md` および同 `detecting-strength-in-code.md`。

## B. 距離チェック（3レンズで別々に見てから統合）

```
□ コード構造: 同一メソッド → 同一クラス → 同一パッケージ/名前空間 → 別サービス → 別システムのどこか
□ 組織構造: 同一チームか別チームか（同じコード構造でも別チームなら実効距離は高い）
□ ランタイム: 同期か非同期か（同期はライフサイクル結合が強く、障害がカスケード）
□ ライフサイクル: 一緒にテスト・デプロイする必要があるか
□ 観察している抽象レベルは何か。そのレベルの境界が「そのレベルでの最大距離」
   （モジュラーモノリス内でもモジュール間結合は高距離問題を起こす）
```

階層と所有権軸の詳細は `balanced-coupling-distance/reference/distance-spectrum.md`。

## C. 変動性チェック

```
□ サブドメイン分類: core（競争優位・継続最適化→最高変動）／supporting（差別化しない→低変動）／
   generic（解決済み問題・既製品あり→機能的変動は低い）のどれか
□ genericの場合: 実装変動性を別途評価（プロバイダ乗り換え・複数実装並行の現実性）
□ 偶発的変動性の除外: コミット頻度が高い＝ドメインが変動的、ではない（設計の悪さが原因かも）
□ 偶発的非変動性の考慮: 変更されていない＝安定、ではない（変えたいのに諦めている可能性）
```

分類の詳細は `balanced-coupling-volatility/reference/domain-classification.md`。git履歴を変動性の証拠にしない理由は `reference/evidence-gathering.md`（正典）。

## D. バランス判定 → Severity

強度と距離が同値（両方高／両方低）なら不均衡。変動性で深刻度を決める。判定表・Severity基準の正典は `reference/imbalance-patterns.md` ——本ファイルでは再定義しない。判定後の指摘は Knowledge Leakage／Complexity Impact／Cascading Changes／Recommended Improvement の4観点で書く（`reference/review-flow.md` Step 4）。

改善レバーは3つ: 強度↓（契約・ACL・ファサード・公開言語）／距離↓（同居・統合）／受容（低変動の実証）。いずれもトレードオフを明記し、具体的な是正立案は balanced-coupling-rebalance に委ねる。

## 実践指針

- 必ず3軸セットで回す。強度だけ・距離だけの指摘を書かない
- 強度は「レベル特定 → 暗黙/明示 → 必要ならコナーセンスで細分」の3段で見る
- 距離はコード・組織・ランタイムの3レンズを混ぜずに別々に評価してから統合する
- 変動性チェックは必ず偶発的変動性/非変動性の除外を通す
- Critical は常に「強×遠×高変動」に限定する。安売りしない

## 出典

- （一次）https://github.com/vladikk/modularity — `skills/balanced-coupling/SKILL.md`（評価観点）, `skills/review/SKILL.md`, `skills/document/SKILL.md`（CC BY-NC-SA 4.0。チェックリスト化は本ファイルでの再構成）

> 帰属注記: 統合強度4段階・3レンズの距離・サブドメイン分類との接続は Khononov オリジナル。古典的モジュール結合度との対応（侵入=content、機能=common/external/control、モデル=stamp、コントラクト=data の精緻化）は Stevens/Myers/Constantine (1974) の分類を下敷きにした Khononov の再解釈。訳語「侵入/機能/モデル/コントラクト結合」は邦訳書由来と推定（「契約結合」の表記揺れあり）。
