# design-for-testability — テスト容易性設計

## 核心
「**テストが書きにくい＝設計が悪いサイン**」。テストの書きにくさは設計を改善する**フィードバック**であり、良い設計＝テストしやすい設計。テスト容易性は「**制御容易性(controllability)**」と「**観測容易性(observability)**」に分解できる。良いユニットテストは **Repeatable（再現可能・何度でも同じ）** かつ **Independent（独立）**。

## 指針
- 本人の言葉「（テストの最初の1つを）選びやすさがいわゆる**テスト容易性**であり、それは**制御容易性**とか**観測容易性**とかに分解できる」。
  - **制御容易性**: テスト対象をテストから駆動・制御しやすいか（DBやネットワークが絡むと下がる）。
  - **観測容易性**: 対象の振る舞い・結果をテストから検証しやすいか（副作用が到達しづらい場所だと下がる）。
  - 講演によっては第3要素として **Small（対象の小ささ・狭いスコープ）** を加えた3要素で語られる（→ `t-wada-tdd-core/reference/tdd-as-design.md`）。矛盾ではなく粒度の違い。
- 制御しづらいもの（**現在時刻**が代表例）への対処。最推奨は「**現在時刻へのアクセスを行うインターフェイスを抽出**し、外部環境との界面にインターフェイスを作りテストダブルで置き換える」（design-for-testabilityで7アプローチを比較）。
- privateメソッドを直接テストしたくなるのは「**責務を持ちすぎているサイン**」。テスト都合で private→public にしない。

## コード例
```
// 悪い: LocalDateTime.now() を本体内で直接呼ぶ → 実行時刻依存でRepeatableでない
// 良い: Clock（時刻提供インターフェイス）を注入し、テストでは固定時刻のダブルを渡す
//       → いつ・何度実行しても同じ結果（Repeatable）
```

## 出典・帰属
- （一次）https://t-wada.hatenablog.jp/entry/design-for-testability 、 https://levtech.jp/media/article/interview/detail_480/ 、 https://swet.dena.com/entry/2023/11/13/170000
- **注記**: design-for-testability記事本文には "controllability/observability" の語は登場しない（本文の柱は Repeatable/Independent と現在時刻7アプローチ）。「制御/観測容易性への分解」はインタビュー(levtech detail_480)が一次。両ソースを組み合わせて理解する。
