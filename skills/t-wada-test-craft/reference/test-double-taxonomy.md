# test-double-taxonomy — テストダブルの5分類

## 核心
テストダブルとは「自動テストに使う偽物・代用品」（映画のスタントダブル＝影武者）。t-wada本人は5分類そのものより「**忠実性(fidelity)を下げる代わりに速度と決定性(determinism)を上げるトレードオフ**」として捉える立場。5分類の原典は Gerard Meszaros『xUnit Test Patterns』、外向き/内向きの整理は t-wada 監訳の Khorikov『単体テストの考え方/使い方』由来。

## 5分類（xUnit Test Patterns）
- **Dummy**：単なる穴埋め（値は使われない）
- **Stub**：間接入力(indirect input)を制御し、任意のデータをSUTに返す
- **Spy**：間接出力(indirect output)を記録し、後で検証する
- **Mock**：間接出力を検証する（期待した呼び出しがされたか）
- **Fake**：依存コンポーネントの実装を軽量な本物相当に置き換える（例: インメモリDB）

## Khorikovの整理（t-wada監訳・推奨）
- **モック＝外向きの（出力の）コミュニケーションの検証**（SUTが依存の状態を変えに行くコマンド）
- **スタブ＝内向きの（入力の）コミュニケーションの代替**（SUTが依存からデータを得るクエリ）
- CQS（コマンド・クエリ分離）に対応。
- **鉄則：スタブに対しては、いかなる通信であっても検証してはならない**（内向き通信の検証はテストを脆くする）。

## 良い例/悪い例
- 良い: 外部APIからの取得値を返すだけの依存は Stub（内向き→検証しない）。メール送信のような外向き副作用は Mock で「送られたか」を検証。
- 悪い: クエリ（内向き）の呼び出し回数まで Mock で検証 → 実装変更で壊れる脆いテスト。

## 出典・帰属
- （一次）https://gihyo.jp/dev/serial/01/savanna-letter/0004 、 https://www.slideshare.net/t_wada/xutp-chapter19
- （監訳経由）Khorikov『単体テストの考え方/使い方』——「モック=外向き/スタブ=内向き」は**Khorikovの枠組みをt-wadaが監訳・紹介**したもの（t-wada独自の言葉ではない）。
- 原典: Gerard Meszaros『xUnit Test Patterns』
