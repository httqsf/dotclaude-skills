# domain-classification — core/supporting/genericサブドメイン分類による変動性評価

このファイルが balanced-coupling-* ファミリー全体における **core/supporting/generic 分類の正典**。他ファミリーからの参照はここに委譲される。

## 核心

未来は予測できないが、DDDのサブドメイン分類は変動性の有用なヒューリスティックを与える。著者は公式プラグインで「**DDDのサブドメインは、変動性の次元を評価する第一の道具である**（DDD's subdomains are the primary tool for evaluating the volatility dimension）」と明言する。論理は競争優位に根ざす: コアサブドメインだけが競争優位を生むため、企業はそこを継続的に最適化・改善し続ける——ゆえに「競争優位を提供するのはコアサブドメインだけなので、それが最も変動的である」（coupling.dev）。

## 分類と変動性の対応（著者自身の整理）

| サブドメイン | 特徴づけ（本人の言葉） | 変動性 |
|---|---|---|
| コア（core） | 「interesting problems」。競争優位・差別化の源泉。競合が模倣しにくい必要があり、問題空間が複雑になりがち | **最高**。継続的に最適化・改善される |
| サポート（supporting） | 「boring problems」。競争優位なし・既製品もなし。典型はCRUDデータ入力やETL | **低** |
| 汎用（generic） | 「solved problems」。既製品・SaaSで解決可能。競争優位なし | **機能的変動性は低いが、実装変動性は可変**（下記） |

注意: 一部の書評は「generic＝高変動」と読める整理をしているが、著者一次ソース（coupling.dev・公式SKILL.md）は「genericは機能的変動性が低い」と明記しており、そちらを採用する。

## 汎用サブドメインの二層分解

汎用サブドメインの変動性は2つに分けて評価する。これは書籍後に著者が公式プラグインで明文化した精緻化（プラグイン独自の精緻化であり、書籍に同一の形で載っている保証はない）:

- **機能的変動性（functional volatility）＝低**: 機能自体は成熟した「解決済みの問題」なのでほぼ変わらない。
- **実装変動性（implementation volatility）＝可変**: プロバイダ乗り換え、別技術の採用、複数実装の並行運用は現実的な変更ベクトル。本人の例——TTSエンジン（AWS Polly）は汎用機能だが、Google TTS・ElevenLabs・セルフホストへの乗り換えは合理的な予想。この場合、プロバイダ固有の知識をカプセル化する強固な統合コントラクトが必須。逆にAuth0のようなID管理は、コントラクト設計に関係なく乗り換えが一大事＝本質的に粘着的（sticky）な実装。
- 指針: 汎用サブドメインの境界を設計するときは「実装を切り替える／複数同時に使う確率」を評価し、それに応じてコントラクト結合を強制する。切り替えコストが本質的に高く可能性も低い場合（IDプロバイダ、コアDB）は実利的ショートカットも許容——ただし変更ベクトルを列挙・検証してからコミットする。

## 分類の運用（著者の公式プラグインの運用形を再構成）

1. 要件・コードからドメイン領域を core/supporting/generic に分類した表（領域／分類／根拠）を**信頼度つきで**提案し、ユーザー／ドメインエキスパートに検証を求める。分類の目的は変動性の決定（「domain classification determines volatility」）。
2. コア＝高変動、サポート/汎用＝低変動として、**設計労力をどこに投資するかを決める**。全域を「よく設計」しようとしない——Evansの「大規模システムのすべてが良く設計されるわけではない」を、著者は変動性で正当化する: サポートサブドメイン＝低変動だからこそ実利的ショートカットが許される（低変動が不均衡を中和する。→ `balanced-coupling-volatility/reference/volatility-in-balance.md`）。
3. 自信の低い分類こそヒアリング対象にする。質問は「答えが分析を変えるものだけ」に絞る。
4. ユーザーがDDD用語を知らない前提で平易に言い換える: コア＝「ビジネスの競争優位を生む部分。組織が積極投資中。頻繁に変わる」、サポート＝「必要だが差別化しない。データ入力・ETL・社内ツール。めったに変わらない」、汎用＝「解決済みの問題。認証・決済・メール配信。機能は成熟、ただしプロバイダや技術は変わりうる」。

## 分類は動く

ビジネス戦略の転換により、サポートサブドメインがコアに昇格し変更率が上がることがある（InfoQ講演での本人の指摘）。分類は一度きりのラベルではなく、ビジネス戦略の分析を継続して更新する。補助として Wardley Maps のコモディティ化軸（genesis→custom→product→commodity）も変動性のもう一つの視点を与える（genesis側ほど変動的）。

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §2.3, §5（公式プラグイン。手順・判定は逐語コピーせず再構成。CC BY-NC-SA 4.0）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/design/SKILL.md, https://raw.githubusercontent.com/vladikk/modularity/main/skills/review/SKILL.md
- （一次）https://coupling.dev/posts/dimensions-of-coupling/volatility/
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/ （戦略転換によるコア昇格）

> 帰属注記: core/supporting/generic のサブドメイン分類は Eric Evans『Domain-Driven Design』(2003) およびDDDコミュニティ由来（Khononov自身も『Learning Domain-Driven Design』第1章で詳述した解説者・発展者）。Khononovの独自貢献は「この分類を変動性推定の第一の道具として転用した」点。「サブドメイン分類＝Khononovの発明」とは書かない。機能的/実装変動性の二層分解は著者の公式プラグインによる精緻化。Wardley Maps は Simon Wardley 由来。
