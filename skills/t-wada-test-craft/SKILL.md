---
name: t-wada-test-craft
description: t-wada（和田卓人）流の「良いテストコードの書き方」を適用する。テストダブルの5分類（ダミー/スタブ/スパイ/モック/フェイク）と外向き=モック/内向き=スタブの整理、テストダブルは極力使わない思想、4フェーズテスト（AAA/Given-When-Then）、テストは仕様書（DAMP over DRY・期待値ベタ書き）、テストデータパターン（Builder/Object Mother）、テスト容易性設計（観測/制御容易性・現在時刻の扱い）を扱う。テストコードを書く・レビューするとき、モックの使い方に迷うとき、テストが読みにくい・仕様が伝わらないとき、現在時刻や外部依存をどうテストするか迷うときに起動。壊れやすい/時々落ちるテストの診断は t-wada-test-reliability が担当（本スキルは処方側）。
---

# t-wada-test-craft — 良いテストコードの書き方

t-wada（和田卓人）が説く、**信頼でき・読みやすく・壊れにくい**テストコードのクラフト。

> 貫く3つの上位メッセージ:
> 1. **テストは設計へのフィードバック**（書きにくい＝設計が悪いサイン）
> 2. **本物を使い、ダブルは境界に最小限**
> 3. **テストは仕様書**

## いつ・どう使うか
- テストが壊れやすい・信用できない → まず `t-wada-test-reliability` で**診断**し、処方（ダブル整理・構造改善）が必要ならここへ戻る
- モック/スタブの使い分けに迷う → `test-double-taxonomy` / `avoid-over-mocking`
- テストの構造を整えたい → `aaa-structure`
- テストが読みにくい・仕様が伝わらない → `tests-as-specification`
- データ準備が煩雑 → `test-data-patterns`
- 現在時刻・外部依存でテストが書けない → `design-for-testability`

## サブトピック（詳細は reference/ 参照）

| トピック | 一言 | 一次の厚さ | 詳細 |
|---|---|---|---|
| テストダブル5分類 | Dummy/Stub/Spy/Mock/Fake。外向き=Mock/内向き=Stub | 厚 | `reference/test-double-taxonomy.md` |
| ダブルを極力使わない | 使わない→作らない→慎重に作る。境界に最小限 | 厚 | `reference/avoid-over-mocking.md` |
| 4フェーズテスト | Setup→Exercise→Verify→Teardown（AAA/GWTと対応） | 薄※ | `reference/aaa-structure.md` |
| テストは仕様書 | DAMP over DRY・期待値ベタ書き | 中※ | `reference/tests-as-specification.md` |
| テストデータパターン | Test Data Builder / Object Mother | 薄※ | `reference/test-data-patterns.md` |
| テスト容易性設計 | 観測/制御容易性・現在時刻はClock注入 | 厚 | `reference/design-for-testability.md` |

※印は「t-wada本人の一次発信が薄い／他者由来」を含む。各reference末尾の帰属注記を参照。

## クイック原則
- **スタブに対しては、いかなる通信も検証してはならない**（内向き通信の検証はテストを脆くする）
- テストダブルの役割は2つ＝「テスト可能化」＋「テストサイズを下げる」
- 期待値は計算せず**ベタ書き**（テスト側で再計算すると同じバグを見逃す＝自作自演）
- privateを直接テストしたくなるのは「**責務を持ちすぎているサイン**」。テスト都合でprivate→publicにしない
- 現在時刻は本体で `now()` を直接呼ばず、**時刻提供インターフェイス（Clock）を注入**する
- 「本物を使う」と「Smallではネットワーク排除」が衝突したら：①設計でSmall化（Humble Object）→ ②本物をコンテナで使いMedium → ③ダブルは境界のみ・最終手段（→ `t-wada-test-strategy/reference/test-ratio-migration.md`）
