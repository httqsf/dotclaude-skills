# balance-formula — BALANCE式（正典）

本ファイルは3軸定義・BALANCE式・「疎結合と高凝集はどちらもバランスした結合の2形態」の**正典**。他のbalanced-coupling-*スキルはここへ委譲する。

## 核心

モジュール性は統合強度と距離が互いに打ち消し合うとき（片方が高く片方が低いとき）に生まれ、両方が同時に高い/低いとき複雑性が生まれる。さらに変動性が低ければ不均衡でも実害がない、という実利性を加えたのが完全形。

## 式（本人の一次ソースで逐語確認済み）

```
MODULARITY = STRENGTH XOR DISTANCE
COMPLEXITY = STRENGTH AND DISTANCE
BALANCE    = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY
```

## 3軸の定義（正典）

- **統合強度（STRENGTH）**: どの種類の知識を共有しているか。連鎖変更が境界を越えて伝播する**確率**
- **距離（DISTANCE）**: 結合したコンポーネントを一緒に変更する社会技術的コスト。連鎖変更の**コスト**
- **変動性（VOLATILITY）**: そもそもコンポーネントに変更が起きる**確率**

## 強度×距離の4象限

| | 低距離 | 高距離 |
|---|---|---|
| **低強度** | **低凝集**（複雑性）— 無関係なものが近くに同居。認知負荷増、泥団子へ漂流 | **疎結合**（モジュール性）— 連鎖変更が少なく高距離が釣り合う |
| **高強度** | **高凝集**（モジュール性）— 共進化は必要だが低距離ゆえ安価 | **密結合**（複雑性）— 頻繁で高価な連鎖変更。分散モノリスへの一歩 |

## 最大の含意（一次で逐語確認済み）

- "Loose coupling and high cohesion are BOTH forms of balanced coupling, not separate concepts."——**疎結合と高凝集は別々の教義ではなく、どちらもバランスした結合の2つの現れ**
- 高凝集の定義: "High cohesion occurs when components that share considerable knowledge are located at low distance, making co-evolution cheap."
- 変動性の役割: Eric Evansの "not all of a large system will be well designed" を引き、実利性（pragmatism）を導入する。高強度×高距離でも変更されないなら低変動性が効果を中和する——技術的には悪い結合だが実害はない

## 二値モデルと数値モデル

公式ブログ・プラグインは説明用に単純化した**二値モデル**を使うと本人が明記している。書籍第10章には連続値を使うより精緻な数値モデルが存在するが、**公開ソースで式は未確認。二値XOR則で判断せよ**（著者の公式スキルも二値運用）。数値モデルの位置づけは測定器ではなく、意思決定を構造化し設計案どうし・変更前後を比較するための道具——スコア単独を公式のように主張しない。

## 実践指針

- 設計レビューでは各依存を4象限にマッピングする。是正最優先は「高強度×高距離×高変動」（分散モノリス象限）。判定表は `balanced-coupling-review/reference/imbalance-patterns.md` へ
- 是正手段は3方向: 強度を下げる（契約化）／距離を縮める（統合）／変動性を確認する（実は変わらないなら放置も合理的）。詳細は `balanced-coupling-rebalance` へ
- 「疎結合にすべき」「凝集を高めるべき」の対立議論を、同じ式の2解として翻訳し合意形成に使う
- 低変動を理由に不均衡を容認したときは、その仮定（変わらないこと）をADRに記録し、変動性が上がったら再均衡する

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §4
- （一次）https://coupling.dev/posts/core-concepts/balance/
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495 ※数値スケールへの言及
- （二次）https://olano.dev/blog/balancing-coupling/

> 帰属注記: BALANCE式と3軸統合はKhononovのオリジナル。式は本人の一次ソースで逐語確認済みで安心して引用可。"not all of a large system will be well designed" はEric Evansの言葉。数値モデルの具体式は書籍本文のみに存在する可能性が高く、Webの本人ソースには登場しない——捏造禁止。
