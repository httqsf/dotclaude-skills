# purpose-driven-abstraction — 目的駆動の抽象化

## 核心
抽象化は「モノ（存在）ベース」ではなく「**目的ベース**」で行うべき。「サバ・サンマ・豚」をモノベースで抽象化すると「魚類／動物」という単なる分類に留まる。しかし目的ベースで「**栄養摂取手段**」という観点で抽象化すると、食べる以外の手段（加工食品・点滴・胃ろう）まで視野に入り、機能革新・イノベーションへの発展性が生まれる。**抽象化の粒度は「何のためか（目的）」で決める**。

## 実践指針
- **目的で抽象化の軸を選ぶ**: 分類（is-a）ではなく「その目的を達成する手段は他に何があるか」で抽象を設計すると拡張性が生まれる。
- **関心の分離で凝集させる**: 曖昧な「商品」を目的ごとに「在庫品」「注文品」「発送品」へ分解。データとロジックが凝集し変更容易性が上がる。
- **過度な抽象化を避ける**: 多目的に使われる汎用クラスは「目的不明オブジェクト（UPO: Unknown Purpose Object）」でありリファクタリング対象。「半端な汎用化」はコストを増やす。
- **抽象の名前も目的特化に**: 抽象化した概念の名前も「意味範囲が狭く、曖昧な解釈を許さない具体的な名前」であること。
- **AI時代も「目的駆動」が軸**: 「目的で駆動する、AI時代のアーキテクチャ設計」として、目的中心の設計思想をアーキテクチャ・AI活用へ拡張。

## コード例（考え方）
```
// 悪い例: モノベースの浅い抽象（分類止まり）
abstract class Fish { }  // サバ/サンマ … 「食べる」以外に広がらない

// 良い例: 目的ベースの抽象（手段が差し替え可能に）
interface NutritionIntake { }         // 栄養摂取手段
class EatingMeal implements NutritionIntake { }   // 食事
class ProcessedFood implements NutritionIntake { } // 加工食品
class IntravenousDrip implements NutritionIntake { } // 点滴
```

## 出典（本人一次情報）
- https://speakerdeck.com/minodriven/buisiness-purpose-system-design 「ビジネス考えてるかい？事業の持続的成長を促進させるシステム設計の考え方」
- https://speakerdeck.com/minodriven/purpose-driven-architecture 「目的で駆動する、AI時代のアーキテクチャ設計」
- https://gihyo.jp/book/pickup/2024/0021 （改訂新版でカプセル化・関心の分離の観点へ全面刷新）
