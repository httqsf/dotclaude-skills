# interface-branch-reduction — 分岐削減 / interfaceによる条件分岐の低減

## 核心
「ロジック単純化の鍵はinterfaceの使いこなし」。型ごとに `if`/`switch` で分岐するコードは、同じ条件判定がコード各所に散らばり保守バグを招く。**interfaceを定義して各実装に同名メソッドを持たせれば（ポリモーフィズム）、呼び出し側は分岐なしで振る舞いだけを差し替えられる**。これにより OCP（拡張に開き修正に閉じる）を満たしやすくなる。

## 実践指針
- **early return（早期リターン）**: 条件を満たさない場合に先に `return` し、「条件ロジック」と「実行ロジック」を分離してネストを解消。「ネストの深淵を見つめる時、深淵もまたお前を見つめている」。
- **switchの重複を撲滅**: 同じ種別で分岐するswitchが複数箇所にあるなら、種別ごとのクラス（オブジェクト）に統合する。
- **interfaceで分岐を消す**: 型判定＋分岐の代わりにinterfaceを定義し、実装クラスに必須メソッドを強制。呼び出し側は `shape.area()` のように分岐なしで多態的に呼ぶ。
- **ストラテジーパターン**: 料金レート・魔法（Fire/Shiden/HellFire）などをinterface実装クラスにし、Map/Listに登録して名前で引く。if/else連鎖を消す。
- **ポリシーパターン**: 複数のルール（条件）を個々のクラスにし、ポリシークラスでまとめて一括評価。ルール追加時に既存コードを触らない。

## コード例
```java
// 悪い例: 型で分岐、同種のswitchが各所に散在
int cost;
switch (magicType) { case FIRE: cost = 2; break; case HELLFIRE: cost = 16; break; ... }

// 良い例: interface + ストラテジー（分岐消去）
interface Magic { int costMagicPoint(); int attackPower(); }
class Fire implements Magic { public int costMagicPoint(){return 2;} ... }
class HellFire implements Magic { public int costMagicPoint(){return 16;} ... }
// 呼び出し側は分岐しない
Map<MagicType, Magic> magics;
int cost = magics.get(type).costMagicPoint();
```

## 出典
- https://sironekotoro.hateblo.jp/entry/2022/06/12/120000 （第6章 ストラテジーパターン）
- https://tgg.jugani-japan.com/tsujike/2022/06/mino6-5/ （第6章 interfaceの応用）
- https://zenn.dev/manase/scraps/bbc605f6497a34
