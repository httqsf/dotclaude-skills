---
name: t-wada-test-strategy
description: t-wada（和田卓人）流の自動テスト戦略「どこに何を書くか」を設計する。Googleのテストサイズ（Small/Medium/Large）、テストピラミッド（アイスクリームコーン/トロフィー/ハニカムをサイズ軸で収束）、スコープ×サイズの3×3マトリクス、Large→Medium→Smallの比率移行戦略、カバレッジを目的化しない扱い方（仕様のカバレッジ・体重計）を扱う。テストの種類・粒度・配分を決めるとき、E2E偏重（アイスクリームコーン）を是正したいとき、CIパイプラインを段階設計するとき、カバレッジ目標の是非を判断するときに起動。
---

# t-wada-test-strategy — 自動テスト戦略：どこに何を書くか

t-wada（和田卓人）が gihyo連載「**サバンナ便り**」で体系化した、自動テストの戦略。曖昧な「単体/統合/E2E」でなく、**客観的な「テストサイズ」で測り直す**のが出発点。

> 貫く目的: 自動テストの目的はコスト削減でなく「**素早く躊躇なく変化し続ける力**」を得ること。戦略はその力を中長期で保つために組む。

## いつ・どう使うか

1. **測り直す** → 既存テストを Small/Medium/Large でラベリング（`test-size-classification`）
2. **形を診る** → サイズ軸で並べ、アイスクリームコーン（E2E過多）になっていないか（`test-pyramid`）
3. **象限を選ぶ** → スコープ×サイズの対角線＝コスパ良好領域を狙う（`scope-size-matrix`）
4. **寄せる** → Large→Medium→Small へ段階的にサイズダウン（`test-ratio-migration`）
5. **測定を道具に** → カバレッジは目的化せず仕様のカバレッジを見る（`coverage-done-right`）

## サブトピック（詳細は reference/ 参照）

| トピック | 一言 | 詳細 |
|---|---|---|
| テストサイズ分類 | S/M/L はリソースアクセス範囲で機械的に決める | `reference/test-size-classification.md` |
| テストピラミッド | サイズ軸で並べると各モデルはピラミッドに収束。SMURF | `reference/test-pyramid.md` |
| スコープ×サイズ | 独立2軸の3×3。対角線が推奨、Large×Unitが反面教師 | `reference/scope-size-matrix.md` |
| 比率移行戦略 | ダブルは「使わない→作らない→慎重に作る」。CUJに絞る | `reference/test-ratio-migration.md` |
| カバレッジの扱い | 100%は100点でない。体重計。仕様のカバレッジ | `reference/coverage-done-right.md` |

## クイック原則
- **Smallを最優先**（速い・安定・並列可・原因特定しやすい）。ネットワークはSmallでは排除
- CIは **Small→Medium→Large** の順（Smallが落ちたら打ち切ってよい）
- 理想は「**サイズを上げずにスコープを広げる**」（複数コンポーネント統合してもSmallのまま）
- E2E/Large は **CUJ（Critical User Journey）** に絞る
- 「**カバレッジに使われるな、カバレッジを使え**」
- 自組織管理下のDB等を安易にモック化しない：①本物をコンテナで使いMedium → ②Humble ObjectでSmall化 → ③ダブルは最終手段（`reference/test-ratio-migration.md`）
