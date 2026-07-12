# invariant-modeling — 不変条件のモデリング

## 核心
モデル自身がデータ完全性を保証できるよう、**制約（不変条件）とロジックをクラス内にカプセル化** する。中核パターンは **「値オブジェクト（Value Object）＋ 完全コンストラクタ」**。完全コンストラクタとは、コンストラクタでバリデーションを行い正常値のみで初期化し、生成時点で「常に正しい状態」のインスタンスだけが存在することを保証する手法。

## 実践指針
- 値オブジェクトは原則1インスタンス変数の単一責任にする。
- コンストラクタで不正値を弾き例外をスローする（生成時点で不変条件を担保）。
- インスタンス変数は `final`／`readonly` で不変にし、初期化後の変更を禁止する。
- 業務概念的に許可された操作のみをメソッドとして公開する。
- 生成方法を制約したい場合はコンストラクタを private にし、用途別の Factory メソッド（例: `Conclude()`, `Reconstruct()`）で生成する。ファーストクラスコレクションでリスト操作もカプセル化する。

## コード例

```csharp
public AmountExcludingTax(int amount) {
    if (!IsValid(amount)) throw new ArgumentOutOfRangeException();
    _amount = amount; // readonly
}
private static bool IsValid(int amount) => 0 <= amount;
```

```java
class Money {
  final int amount;          // 不変
  final Currency currency;   // 不変（非null）
  Money(final int amount, final Currency currency) {
    if (amount < 0) throw new IllegalArgumentException();
    if (currency == null) throw new IllegalArgumentException();
    this.amount = amount; this.currency = currency;
  }
}
```
`Money.add()` は元インスタンスを変更せず、通貨一致を検証したうえで新しい `Money` を返す（不変性の維持）。

## 出典
- https://qiita.com/MinoDriven/items/5e69d9bd028aa350e2c4 （設計要件をギッチギチに詰めたValueObjectで低凝集クラスを爆殺する）
- https://speakerdeck.com/minodriven/data-destroy-driven （破壊せよ！データ破壊駆動で考えるドメインモデリング）
