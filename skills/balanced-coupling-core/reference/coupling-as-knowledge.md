# coupling-as-knowledge — 結合＝知識共有

## 核心

結合とは2つのコンポーネント間のあらゆる依存関係であり、その本質は「知識の共有」である。統合にはコンポーネント同士が互いについての知識（機能・公開インターフェース・実装詳細）を交換することが必要で、共有される知識が多いほど、一方の変更が他方への連鎖変更を引き起こす確率が上がる。そして結合は排除すべき悪ではない——"Coupling is what makes the value of a system greater than the sum of its parts."（結合こそがシステムの価値を部分の総和より大きくする）。

## 本人の言葉（一次ソースで逐語確認済み）

- "Coupling is any dependency between two components of a system. The design of that dependency defines the likelihood of a change in one component requiring a corresponding change in the other."（coupling.dev）
- "You cannot build a system out of fully independent components — that would go against the very definition of 'system.' The question is not whether to couple, but *how* to couple."（vladikk/modularity SKILL.md）
- 水の比喩（InfoQ登壇）: 結合は水と同じ。必要不可欠だが、過剰摂取は害になる。

書籍第1章の趣旨として: 設計とは「どの知識の流通を許可し、どの知識を境界の外に出さないか」を決める行為である（※趣旨。英語原文の逐語引用ではない）。

## 実践指針

- 「疎結合にせよ」ではなく「この依存で何の知識が共有されているか」を最初に問う（テーブル構造か、処理順序か、ドメインモデルか、契約だけか）
- 依存の有無・本数を数えるのではなく、依存の「設計」（＝共有知識の量と性質）を評価する
- 結合ゼロを目標にしない。コンポーネントが完全独立ならそれはもはやシステムではない
- モジュール境界を決めるとは「外に流してよい知識」と「絶対に漏らさない知識」を列挙する行為だと捉える
- 暗黙の知識共有（重複実装された業務ルール、非公開APIの利用）を最も警戒する。明示的な共有より発見が遅れ、壊れやすい

## 具体例

- 注文サービスが決済サービスのDBテーブルを直接 `UPDATE` する → テーブル名・カラム・状態値・状態遷移という大量の知識を暗黙に共有（最強の統合強度）
- フロントエンドとバックエンドに同じバリデーションルールを重複実装 → コード上の参照が無くても知識を共有している。仕様変更時に両方を同時変更しないと不整合

```typescript
// 悪い例: 他サービスの実装詳細（テーブル構造・状態値）という知識を共有
await db.query("UPDATE payments SET status = 3 WHERE order_id = ?", [orderId]);

// 良い例: 共有する知識を契約（公開インターフェース）まで削る
await paymentsClient.cancelPayment(orderId);
```

（他言語でも同等の考え方が適用できる）

強度の4分類（侵入/機能/モデル/コントラクト）の詳細は `balanced-coupling-strength/reference/integration-strength-four-levels.md` へ。

## 出典

- （一次）https://coupling.dev/posts/core-concepts/coupling/
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495 ※読者解説
- （二次）https://olano.dev/blog/balancing-coupling/ ※章別要約

> 帰属注記: 「結合＝知識共有」の定式化と3軸への展開はKhononovオリジナル。「許可・禁止する知識の流れを決めるのが設計」は書籍第1章由来の趣旨として提示可（逐語引用不可）。書籍本文は未取得のため、引用は本人のWeb一次ソースで確認済みのものに限る。
