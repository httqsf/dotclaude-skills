# fractal-geometry — 設計のフラクタル性

## 核心

ソフトウェア設計はフラクタルであり、同じBalanced Couplingモデルが、メソッド内の変数からオブジェクト、モジュール、サービス、システム、組織間の相互作用まで、あらゆる抽象化レベルに繰り返し適用できる——"Whether it's methods within an object or microservices in a distributed system, the underlying ideas are the same."（InfoQ）。ただし各レベルで「何が契約で何が実装詳細か」「何が最短/最長距離か」の意味はシフトするため、モデル適用前に必ず「どの抽象化レベルを観察しているか」を確定しなければならない。

## 本人の言葉（一次ソースで逐語確認済み）

- "Software design is fractal. You design components and their interactions at different levels of abstraction — from objects to modules to services to interactions across entire systems. The balanced coupling model applies at all of these levels."（vladikk/modularity SKILL.md §3）
- 意味のシフト: "An integration contract at a lower level of abstraction (e.g., a class's public interface) may be an implementation detail at a much higher level (e.g., within a service boundary)."——低レベルの契約は高レベルでは実装詳細
- 距離の相対性: "At any given level of abstraction, the highest distance is the boundary of that level."——観察レベルの境界がそのコンテキストでの最長距離。**マイクロサービスがなくても高距離結合の問題は起こる**
- "Without this framing, the same coupling relationship can appear balanced or unbalanced depending on the observer's perspective."（レベルを確定しないと同じ結合が均衡にも不均衡にも見える）

## 適用スケールの系列（書籍第12章 "Fractal Geometry of Software Design"）

```
変数 ↔ 関数
関数 ↔ クラス
クラス ↔ モジュール
モジュール ↔ アプリケーション
サービス ↔ サービス
システム ↔ 組織（チーム・会社間の知識共有まで）
```

どのレベルでも「何を共有しているか（強度）」「どれだけ離れているか（距離）」「どれだけ変わるか（変動性）」を評価できる。

## 実践指針

- モデル適用前に必ず宣言する: 「今、観察している抽象化レベルは何か」。それにより (a) 何が契約/モデル/機能/侵入に当たるか、(b) 最短・最長距離が何か、が決まる
- クラス設計の議論とサービス分割の議論に同じ3軸語彙を使い、チームの設計言語を統一する
- モノリスだからと油断しない。モジュールレベルではモジュール間が最長距離であり、そこでの高強度結合は立派な不均衡
- レベルを跨いだ議論の混線（「このDTOはサービスレベルでは契約だがクラスレベルではモデル」等）を明示的に解きほぐす
- 組織レベルにも適用: チーム間で共有される知識の量（強度）×チーム間距離×事業の変動性で、Conway的な組織設計を評価する

## 具体例

- クラスのpublicインターフェース: クラスレベルでは「コントラクト結合」の道具だが、サービス内部の話であればシステムレベルでは全部「実装詳細」
- 単一リポジトリ・単一デプロイの個人プロジェクトでも、`billing` モジュールと `orders` モジュールが互いの内部モデルを共有していれば、そのレベルにおける最長距離×高強度＝不均衡（TypeScriptに限らず他言語でも同等）

フラクタル性は古典理論の再利用も正当化する——1970年代の構造化設計文献について "The problems that they're facing... are also going to be quite familiar once you step over those terms."（用語を乗り越えれば同じ問題）。

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §3
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （一次・本人登壇）https://speakerdeck.com/vladikk/fractal-geometry-of-software-design / https://www.youtube.com/watch?v=1ZyR_tgGTp8（DDD Europe 2022）
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495 ※第12章解説

> 帰属注記: 設計のフラクタル性とBalanced Couplingモデルの全レベル適用はKhononovのオリジナル主張（本人登壇タイトルにもなっている）。Conwayの法則はMel Conway由来で、組織レベル適用の根拠として本人が引用。
