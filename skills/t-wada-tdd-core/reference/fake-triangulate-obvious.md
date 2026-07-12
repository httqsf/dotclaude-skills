# fake-triangulate-obvious — 仮実装・三角測量・明白な実装

## 核心
Greenに至る道具は1つではなく、**自分の不安（自信）の大きさに応じて3つの戦略を使い分ける**。「大事なのは自分の不安をコントロールすること」。自信が高ければ一気に、低ければ小さく刻む。TDDのスキルとは、この歩幅を自在に変えられること。

（原典はKent Beck『テスト駆動開発』第2部。t-wadaは訳者・解説者。）

## 3戦略
- **仮実装（Fake It）**: テストを通すためのベタ書き（定数を返すだけ `return 1;`）。狙い①「動作するコード」を最短で得る、②**テストのテスト**（ベタなGreenでも緑にならなければバグはテスト側）。不安が大きいときの最小の一歩。
- **三角測量（Triangulation）**: 別のデータを使ったテストを足して仮実装を破綻させ、一般解を導く。語源は測量（2点から交点に解を定める）。正しい一般化が難しい・不安なときの最も慎重な進め方。
- **明白な実装（Obvious Implementation）**: 実装が自明なら仮実装や三角測量を経ず、いきなり正しいコードを書く。自信があるときの最速ルート。外して赤が出たら歩幅を狭める。

## コード例（Stack）
```java
// 仮実装：まず定数でGreen
public int size(){ return 1; }

// 三角測量：2件目のデータを足してフェイクを壊す
s.push(1); assertEquals(1, s.size());
s.push(2); assertEquals(2, s.size()); // ← return 1 では通らない → 一般解へ
public void push(int v){ ++size; }
public int size(){ return size; }

// 明白な実装：自明なので一気に
public boolean isEmpty(){ return size == 0; }
```

## 出典
- （一次）https://t-wada.hatenadiary.jp/entry/20100228/p1 「RSpecの入門とその一歩先へ」
- 原典: Kent Beck『テスト駆動開発』（t-wada訳）第2部
- （コード例参考・非t-wada）https://objectclub.jp/technicaldoc/testing/stack_tdd.pdf
