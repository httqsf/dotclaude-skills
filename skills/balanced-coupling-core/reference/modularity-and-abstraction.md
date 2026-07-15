# modularity-and-abstraction — モジュール性と抽象化

## 核心

モジュール性は複雑性の正反対——"Modularity is a very strong relationship between an action and its outcome."（モジュール性＝行動と結果の強い関係）。モジュール設計が満たすべき2基準は "1. It is clear what parts of the system need to change. 2. The outcome of the change is clear and predictable."（どこを変えるべきかが明確／変更の結果が明確で予測可能）。モジュールとは単なるフォルダやデプロイ単位ではなく、明確に定義された機能を境界づけ、公開インターフェースを通じて公開し、それ以外の知識を隠蔽する境界である。

## 本人の言葉（一次ソースで逐語確認済み）

- "Modularity extends a system's goals into the future. It doesn't mean preemptively implementing future requirements — that's impossible. It means making the implementation of *reasonable* future changes easy by minimizing the cognitive load required."（モジュール性＝システムの目標を将来に延長すること。将来要件の先回り実装ではない）
- "Even a 'big ball of mud' can achieve its current goals."（泥団子でも現在の目標は達成できる。差が出るのは将来）

## 抽象化＝新しい意味レベル

抽象化とは「物事を曖昧にすること」ではなく、下位の詳細を隠して新しい意味レベル（semantic level）を作り出す行為。公開インターフェースが実装用語ではなくドメイン用語で語れるかが試金石。

## 実践指針

- モジュールごとに「隠蔽する知識」を文章で書けるか点検する。書けなければそれは単なるフォルダ分割
- 公開インターフェースが「新しい意味レベル」を提供しているか確認する（実装用語でなくドメイン用語で語れるか）
- deep moduleを目指す: 小さなインターフェースの背後に大きな機能・知識を隠す（Ousterhout由来の基準として明示的に使う）
- モジュール性への投資は「合理的に起こりうる将来の変更」の認知負荷削減として説明する。将来要件の先回り実装とは区別する
- 「変更すべき場所が明確か」「変更の結果が予測可能か」の2基準でモジュール境界をレビューする

## 具体例

決済モジュールが公開してよいのは契約だけ。漏らしてはならないのはStripe API仕様・再試行アルゴリズム・状態遷移・冪等性キー生成・テーブル構造。外部が内部知識なしで使えるほどモジュールは「深い」。

```typescript
// 公開する知識（契約）— ドメイン用語の新しい意味レベル
interface Payment {
  authorize(orderId: OrderId, amount: Money): Promise<PaymentId>;
  refund(paymentId: PaymentId, amount: Money): Promise<void>;
}
// 隠蔽する知識: Stripe API仕様 / リトライ戦略 / 状態遷移 / DBスキーマ
```

（他言語でも同等の考え方が適用できる）

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §1.3–1.4
- （一次）https://coupling.dev/posts/core-concepts/modularity/ ※高レベル概説
- （二次）https://olano.dev/blog/balancing-coupling/ ※第4章の帰属情報
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495

> 帰属注記: モジュール設計の2基準・「目標を将来に延長する」はKhononov（一次で確認済み）。情報隠蔽はDavid Parnas (1972) 由来。**deep moduleはJohn Ousterhout『A Philosophy of Software Design』由来**でKhononovのオリジナルではない（書籍第4章はParnas/Myers/Brooks/Conway/Ousterhoutに言及）。「新しい意味レベル」の言い回しは元来Dijkstraの階層抽象の思想圏で、本文中の帰属表記は未確認——Khononovの独創として断定しない。
