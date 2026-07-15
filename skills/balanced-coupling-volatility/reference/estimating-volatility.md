# estimating-volatility — 変動性の推定方法

## 核心

変動性はコミット履歴やソースコードからは推定できない。著者の公式プラグインは変動性の評価手段を「DDD subdomains / Wardley Maps / **(NOT commit history)**」とわざわざ括弧書きで明記する。InfoQ講演での本人の言葉: 「コンポーネントの変更率を本当に予測するには、自分の経験やソースコードを見るだけでは足りない」「ビジネスドメインを理解しなければならない。ビジネス戦略を分析しなければならない」。

## なぜ git 履歴を変動性の証拠にしないか

観測される変更頻度は両方向に嘘をつく:

- **コミットが多い ≠ 高変動ドメイン**: 観測値は本質的変動性＋偶発的変動性（設計不良による連鎖変更）の合算。著者のdesignスキルの制約: 「High commit frequency may indicate poor design (accidental volatility), not a volatile domain.」
- **コミットが少ない ≠ 安定**: 静けさは偶発的非変動性（怖くて触れないだけの見かけの安定）を含む。ビジネスは変えたいのに設計の脆さが変更を妨げているなら、それは低変動ではなく機会損失。

ただし git 共変更分析（一緒に変更されるファイル・同時デプロイが必要なサービスの発見）は**暗黙的結合の発見器**としては有効。その正しい使い方の正典は `balanced-coupling-review/reference/evidence-gathering.md` に委譲する。本ファミリーの観点は一つだけ: 共変更分析で見つかった高頻度変更は、必ずビジネス視点で「本質的か偶発的か」に仕分けること。

## 推定の情報源（優先順）

1. **ビジネス視点（本人が第一に挙げるもの）**
   - サブドメイン分類: 競争優位を生む領域か（コア＝高変動）。→ `balanced-coupling-volatility/reference/domain-classification.md`
   - ビジネス戦略: 今後の投資領域・戦略転換・計画中の移行（著者のreviewスキルは「Strategic direction」を必須ヒアリング項目にしている）。
   - ビジネスルールの不確実性: 仕様がまだ探索段階か。新規事業領域は本質的に高変動（Cynefinの複雑系＝実験して学ぶ領域とも呼応）。
   - Wardley Maps のコモディティ化軸: genesis側ほど変動的、commodity側ほど安定。
2. **外部要因**: 法改正・規制・外部API仕様変更の頻度。汎用サブドメインではプロバイダ乗り換え・複数実装並行の確率（実装変動性）。
3. **組織シグナル**: 複数チームから変更要求が来る領域か（変更圧力の集中点）。「変更が予想外に高くつく場所・デプロイが物を壊す場所」のヒアリング。
4. **履歴データ（補助。単独では不十分）**: 共変更分析は距離×変動性の現状把握に使い、本質/偶発の仕分けと必ず突き合わせる。

## 実践指針

- 変動性推定は「git logを見る作業」ではなく「ビジネスを理解する作業」。ドメインエキスパート・PdMへのヒアリング（競争優位はどこか、今後どこに投資するか）を一次情報にする。
- 統合ごとに「ビジネスドメインの視点から、この領域はどのくらい変わりそうか」を問う。汎用サブドメインは機能的変動性と実装変動性を区別する。
- 最低限のチェック項目: (1) 競争優位に直結するか (2) 仕様・ビジネスルールはまだ不確実か（新規事業か） (3) 法改正・外部仕様変更に晒されるか (4) 複数チームから変更要求が来るか (5) 過去に一緒に変更されてきたか（補助）。
- 静かなコンポーネントには「ビジネスは本当は変えたいのに触れないだけでは？」と問う（偶発的非変動性の検出）。
- 見積もりは近似でよい。目的は優先順位付け（どの結合の不均衡から直すか）であり、精密な予測ではない。

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md §2.3, §6（「NOT commit history」の明記）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/review/SKILL.md, https://raw.githubusercontent.com/vladikk/modularity/main/skills/design/SKILL.md
- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （一次）https://coupling.dev/posts/dimensions-of-coupling/volatility/

> 帰属注記: 「ビジネスドメインから見積もる／NOT commit history」はKhononov本人の枠組み（公式プラグイン・講演）。Cynefin は Dave Snowden、Wardley Maps は Simon Wardley 由来の借用。共変更分析の運用手順の正典は `balanced-coupling-review/reference/evidence-gathering.md`。
