# coupling-and-complexity — 結合と複雑性

## 核心

複雑性とは「行動（変更）とその結果の関係が予測不可能な状態」——"If the relationship between cause and effect can only be identified in hindsight, that's complexity."（因果関係が事後にしか特定できないなら、それが複雑性だ）。しかも複雑性は客観量ではなく `complexity = f(system design, cognitive limits)`、システム設計と人間の認知限界の関数である。複雑性とモジュール性は正反対の性質だが、どちらも同じ原因＝結合の設計から生まれる。

## Cynefinによる判定（Khononovの転用）

| 領域 | 変更の結果は… |
|---|---|
| Clear（明確系） | 正確にわかる |
| Complicated（煩雑系） | 自分にはわからないが専門家ならわかる。相談すればよい |
| Complex（複雑系） | 変更して観察する以外に知る方法がない。因果は事後にしか特定できない |
| Chaotic（混沌系） | 事後ですら行動と結果の関係を特定できない |

- "The only way to determine the outcome is to make the change and observe what happens."（Complex領域の定義、vladikk/modularity SKILL.md）
- モジュール設計の目標＝「変更の影響が事後にしかわからない状況」を避けること
- 変更に必要な情報が認知容量（4±1チャンク。1950年代の7±2から後年下方修正）を超えるとき、その設計は複雑

## 自由度と制約・local/global複雑性

- 複雑性は規模（コード量）と無関係で、要素間の相互作用と自由度から生じる。無制限の柔軟性・自由度は複雑性の源であり、効果的な制約が自由度を落として複雑性を下げる——**制約＝設計**
- **local complexity**（コンポーネント内部の相互作用）と **global complexity**（コンポーネント間の相互作用）を区別する。Khononov自身のマイクロサービス移行失敗の自己診断は "traded local complexity into global complexity"（InfoQ）——ローカル複雑性をグローバル複雑性に交換しただけ

## 実践指針

- 複雑性の判定質問: 「この変更をしたら何が起きるか、実行前に言えるか？」言えなければComplex
- 変更に必要な知識が4±1チャンクを超えていないかで境界設計を点検する
- 自由度を減らす制約（書き込み経路の限定・状態遷移の限定・公開APIの限定）を「不便」ではなく「複雑性の削減」として正当化する
- 分割時はlocal→globalの複雑性移転に注意。境界を増やすだけでは複雑性は消えず移動する
- 複雑性は主観的（観察者の認知に依存）なので、チームの習熟度・ドメイン知識も設計判断の変数に含める

## 具体例

どこからでもDBを書き換えられるシステム（Controller/Batch/Lambda/管理画面/外部ツール→DB）は自由度最大＝原因追跡が不可能に近い。書き込み経路をユースケース層に限定する制約で予測可能性が回復する。（他言語・他アーキテクチャでも同等の考え方）

## 出典

- （一次）https://coupling.dev/posts/core-concepts/complexity/
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §1.2
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （公式目次）https://www.pearson.de/media/muster/toc/toc_9780137353538.pdf
- （二次）https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495 / https://olano.dev/blog/balancing-coupling/

> 帰属注記: 複雑性の定義はDave SnowdenのCynefinフレームワーク（Complex領域）のソフトウェア設計への転用（Khononov自身が明示）。認知容量7±2/4±1は心理学研究（Miller 1956 / Cowan 2001系）由来で本人は引用者。"Software design is a constant battle with complexity." はEric Evansの言葉としてcoupling.devが引用。`complexity = f(system design, cognitive limits)` は本人の式（一次で確認済み）。
