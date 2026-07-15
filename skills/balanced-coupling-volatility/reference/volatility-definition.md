# volatility-definition — 変動性の定義とParnasの系譜

## 核心

変動性（volatility）とは「コンポーネントがそもそも変更を必要とする確率（the probability of components needing to change in the first place）」（著者ブログ coupling.dev）。統合強度が「変更が境界を越えて伝播する可能性」、距離が「連鎖変更のコスト」を表すのに対し、変動性は時間の次元を担う——本人はInfoQ講演で「第3の次元は時間の次元、変動性の次元だ。なぜ結合を気にするのか？　システムを変更できるようにしたいからだ」と述べている。変動性が高いほど、設計上の問題はより深刻で「痛い」ものになる（"The higher the volatility, the more acute and 'painful' design issues will be."）。

## Parnasの系譜と貢献の切り分け

- 原典はParnas「On the Criteria To Be Used in Decomposing Systems into Modules」(1972)。「各モジュールは、難しい設計判断と、変わりやすい（likely to change）設計判断を隠すように設計せよ」＝情報隠蔽。
- Parnasは「変わりやすいものを隠せ」と述べたが、**何が変わりやすいかをどう見積もるか**の体系的手法は示さなかった。Khononovの貢献は (a) 変動性を統合強度・距離と並ぶ独立の第3軸に定式化したこと、(b) DDDサブドメイン分類という見積もりヒューリスティックを与えたこと（→ `balanced-coupling-volatility/reference/domain-classification.md`）、(c) バランス式に組み込み「変わらないなら不均衡でも許容」を式で表現したこと。
- 本人はTech Lead Journal #188で、抽象化の判断基準として「David Parnasの言葉で言えば、何らかの情報を隠す（hide some information）ことに役立つか」とParnasの語彙を明示的に使っている。

## 3つの変動性概念（本質的／偶発的／偶発的「非」変動性）

| 概念 | 意味 | 見たときの対処 |
|---|---|---|
| 本質的変動性（essential volatility） | ビジネスドメイン自体が本当に変化するために生じる変更。設計ではなくビジネスの性質に由来 | 高変動として設計投資（強度×距離のXORバランス厳守） |
| 偶発的変動性（accidental volatility） | 統合強度と距離の管理に失敗した設計のせいで、本来変わる必要のないコンポーネントが頻繁に変更される状態 | 直すべきは変動性ではなく統合強度・距離の設計 |
| 偶発的非変動性（accidental involatility） | 一見安定して見えるが、本質的に安定なのではなく、劣悪な設計ゆえの高いリスクと労力が変更を妨げている状態。InfoQ講演では「ビジネスがそこに触るのを恐れている」と表現 | 「安定」と誤認しない。最適化したい業務領域が放置されている＝**機会損失のシグナル** |

essential/accidental の用語は著者自身の筆（coupling.dev と公式プラグインSKILL.md）で確認済みの本人用語。Brooks『銀の弾丸はない』の essence/accident 二分法を変動性に適用したもの（概念として提示する。書籍の逐語引用として扱わないこと）。

## 実践指針

- 結合を評価するとき、「この依存はそもそもどのくらいの確率で変更されるのか」を統合強度・距離と独立に問う。変動性を無視した結合評価は片手落ち。
- 変更頻度が高いコンポーネントを見たら、まず「本質的か偶発的か」を仕分ける。偶発的なら変動性ではなく設計（強度・距離）を直す。
- 変更頻度が低いコンポーネントを見たら「本当に安定か、怖くて触れないだけか」を疑う。後者はビジネス機会の損失。
- 「変わりやすい判断を隠せ」（Parnas）を実行するには変わりやすさの見積もりが先。コードの現状ではなくビジネスドメインから見積もる（→ `balanced-coupling-volatility/reference/estimating-volatility.md`）。
- 見積もりは予言ではなくヒューリスティック。「合理的に予見できる変更」を楽にするのが目的で、将来要件の先回り実装ではない。

## 出典

- （一次）https://coupling.dev/posts/dimensions-of-coupling/volatility/ （著者ブログ・定義と essential/accidental）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §2.3（公式プラグイン・偶発的非変動性）
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/ （本人講演）
- （一次）https://techleadjournal.dev/episodes/188/ （本人インタビュー・Parnas言及）
- （二次）https://olano.dev/blog/balancing-coupling/ ※書籍tl;dr。「Volatility is a component's expected rate of change.」の引用元
- （古典）Parnas (1972) "On the Criteria To Be Used in Decomposing Systems into Modules"

> 帰属注記: 情報隠蔽と「likely to change」はParnas (1972) が原典。変動性の第3軸への定式化、essential/accidental volatility、偶発的非変動性（accidental involatility）はKhononovオリジナル。書籍本文は未取得のため、引用は本人の公開文書・発言由来であり書籍の逐語ではない。
