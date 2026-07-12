# purpose-centered-encapsulation — 目的中心のカプセル化

## 核心
クラス設計とは「**インスタンス変数を不正状態に陥らせないためのしくみづくり**」である。データ（値）とそれを操作するロジックを同じクラスに凝集させ、外部から不正な状態変更を防ぐことがカプセル化の本質。「強く関係するものはカプセル化、関係が弱いものは関心の分離」で構造を決める。

## 実践指針
- **完全コンストラクタ**: インスタンス変数を全て初期化できる引数を持ち、コンストラクタ内のガード節で不正値を弾く。生成時点で必ず正常なオブジェクトにする。
- **値オブジェクト（ValueObject）**: 金額・日数などをプリミティブ型でなくクラスとして表現し、バリデーションとロジックをそのクラスに凝集させる。型が違えば「値の渡し間違い」がコンパイルで防げる。
- **不変（イミュータブル）**: setterを設けず、状態変更は「新しいインスタンスを生成して返す」ことで表現。予期せぬ副作用を防ぐ。
- **ファーストクラスコレクション**: コレクション（List等）とそれを操作するロジックを専用クラスにまとめる。外部へ渡す際は変更不可にする。
- **getter/setter回避**: 生のデータを露出せず、「〜する」という振る舞い（メソッド）でのみ操作させる。`〜情報`／`〜データ` という名前は「ドメインモデル貧血症」の兆候。

## コード例（Java風）
```java
// 悪い例: 可変・getter/setterで不正状態を許す
class Money { public int amount; } // 負の金額も入れられる

// 良い例: 完全コンストラクタ + 値オブジェクト + 不変
class Money {
    final int amount;
    Money(int amount) {
        if (amount < 0) throw new IllegalArgumentException("金額は0以上");
        this.amount = amount;
    }
    Money add(Money other) { return new Money(this.amount + other.amount); } // 新インスタンスを返す
}
```

## 出典
- https://qiita.com/ryamate/items/de234799b65b13d7a117 / https://ryamate.hatenablog.com/entry/web-engineer-code-design-tips-3-2 （第3章 クラス設計）
- https://qiita.com/KIRIN3qiita/items/62e6ffc15cf114fa08e4
