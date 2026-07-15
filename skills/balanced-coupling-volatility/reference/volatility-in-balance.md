# volatility-in-balance — バランス式における変動性の役割

## 核心

バランス式 `BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY` において、変動性は「実利（プラグマティズム）の項」。強度と距離が打ち消し合っていれば（XOR＝片方だけ高い）モジュール性が成立するが、両方高い不均衡でも**変動性が低ければ `NOT VOLATILITY` が真になり、全体としてバランスは成立する**。著者の公式プラグインの原文: 「A legacy system with high intrusive coupling that never evolves is tolerable — the coupling is technically "bad" but practically harmless.（結合は技術的には『悪い』が、実害はない）」。式・XOR含意の詳細な正典は `balanced-coupling-core/reference/balance-formula.md`。本ファイルは変動性項の解釈のみ扱う。

## NOT VOLATILITY の意味

- 変動性は問題の「有無」ではなく「痛みの発現確率」を決める増幅器。不均衡な結合は地雷だが、誰も踏まなければ（変更が来なければ）爆発しない。InfoQでの本人の表現: 遠距離で知識を共有していても両者が変わらない限り問題にならない。一方が変更を迫られ他方に影響するなら「分散した泥団子（distributed ball of mud）」問題になる。
- 逆方向の含意が重要: **高変動領域では `NOT VOLATILITY` が偽になるため、強度と距離のXORバランスが必須**。高い知識共有が必要なコンポーネントは同居させる（高凝集）、遠距離のコンポーネントはコントラクトで統合（疎結合）。低変動コンポーネントでは実利的ショートカットが許容できる。
- 書籍第10章には連続値の数値モデルがあるが、公開ソースで式は未確認。二値XOR則で判断せよ（著者の公式スキルも二値運用）。式は測定器ではなく、設計議論を構造化する共通言語として使う。

## 本人公認の実例: 変わらないレガシー上流ならDB直読みでも構わない

Tech Lead Journal #188での本人の発言: その上流システムが決して変わらないレガシーシステムなら「そのデータベースから直接データを取っても構わない（it's fine to take its data from its database）」——侵入結合（最強の統合強度）＋遠距離という最悪の組み合わせでも、上流の変動性が低いから許容される。低変動が不均衡を中和する最も鮮烈な例。

同じ論理で、**強×遠×低変動 = tolerable technical debt（許容できる技術的負債）**——記録はするが優先しない。強×遠×高変動は Critical（変更が頻繁・高価・予測不能になる）。不均衡パターンの判定表の正典は `balanced-coupling-review/reference/imbalance-patterns.md`。

## 許容の条件（NOT VOLATILITY を乱用しない）

- 「低変動」は検証済みの仮定であるべき。偶発的非変動性（怖くて触れないだけの見かけの安定。→ `balanced-coupling-volatility/reference/volatility-definition.md`）を低変動と誤認すると、地雷を「安全」とラベリングすることになる。
- 変動性は時間とともに変わる。戦略転換でサポート→コア昇格すれば、許容していた不均衡が突然Criticalになる。低変動を理由に許容した箇所は、前提（なぜ低変動と判断したか）と再評価トリガー（どうなったら見直すか）をADR等に記録する。
- 「許容できる」は「良い設計」ではない。フラグは立てる（Note it）——見えなくするのではなく、優先順位を下げるだけ。

## 実践指針

- リファクタリング優先順位は「強い×遠い×高変動」を最優先に。「強い×遠い×低変動」は記録して劣後。
- 変わらないレガシー・外部システムとの統合では、疎結合化（ACL・コントラクト層）への投資を必須としない。変動性が本当に低いなら侵入結合すら実利的に許容できる（本人公認）。
- 逆にコアドメイン（高変動）ではXORバランスを厳格に: 知識を多く共有するものは近くに、遠くに置くものはコントラクトのみ共有。
- 「低変動だから許容」の判断は根拠＋再評価トリガーをセットで記録する。
- 論理式を客観的品質スコアとして振りかざさない。

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §4.3, §5
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/review/SKILL.md Step 3（優先度分類）
- （一次）https://techleadjournal.dev/episodes/188/ （レガシーDB直読み許容の本人発言）
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/, https://www.infoq.com/podcasts/balancing-coupling-software-design/
- （二次）https://olano.dev/blog/balancing-coupling/, https://kalabro.tech/balancing-coupling-in-software-design-book/ ※書評

> 帰属注記: BALANCE式・`NOT VOLATILITY` による中和・tolerable technical debt の整理はKhononovオリジナル。式の正典は `balanced-coupling-core/reference/balance-formula.md`、不均衡判定表の正典は `balanced-coupling-review/reference/imbalance-patterns.md`。数値モデル（0-10スケール）は書籍のみに存在し具体式は公開ソース未確認のため記載しない。
