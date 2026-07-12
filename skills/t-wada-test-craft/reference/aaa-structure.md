# aaa-structure — 4フェーズテスト（AAA / Given-When-Then）

## 核心
t-wada の一次発信で軸になるのは、xUnit Test Patterns 由来の **Four-Phase Test（4フェーズテスト）**：**Setup（準備）→ Exercise（実行）→ Verify（検証）→ Teardown（後片付け）**。この定型構造を守ること自体が「**ワンパターン＝読みやすさ＝Tests as Documentation**」につながる。Arrange-Act-Assert / Given-When-Then はこの4フェーズと対応する（Setup≈Arrange/Given、Exercise≈Act/When、Verify≈Assert/Then）。

## 指針
- 各テストは4フェーズの定型に沿って書き、構造を揃える（読み手の認知負荷を下げる）。
- 目指すのは「**テストメソッド毎にアサーションひとつ**」。1テストで多数を検証する Assertion Roulette を避ける。
- 共通の準備は setup に抽出し、テスト毎に違う部分だけを書く（ただしDAMPとのバランス＝`tests-as-specification` 参照）。
- Given-When-Then / Gherkin記法はビジネス側との共有に有効だが、全員同席のワンチームでない場合は効果限定的、と現実的に評価。

## 良い/悪い
- 良い: Setup→Exercise→Verify→Teardown が視覚的に分かれ、Verifyのアサーションが1つに絞れている。
- 悪い: ループ内アサーションや1メソッドに複数の検証観点が混在（どこで落ちたか分からない）。

## 出典・帰属（重要）
- （一次）https://www.slideshare.net/t_wada/xutp-chapter19 （Four-Phase Test）、 https://t-wada.hatenablog.jp/entry/design-for-testability （Assertion Roulette回避）
- **注記**: t-wada本人がAAA/GWTを一次で強く語った発信は薄い。彼の語彙は「**Four-Phase Test**」寄りで、"Arrange-Act-Assert"/"Given-When-Then" という語自体はスライドに明示されていない（4フェーズとの対応は整理）。SkillでAAA/GWTを扱う際は「Four-Phase Testと対応する一般用語」と理解する。
- 原典: Meszaros『xUnit Test Patterns』Four-Phase Test
