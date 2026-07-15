# review-flow — レビューフロー（4ステップと情報源の分担）

## 核心

Khononov自身がエージェント向けに設計したレビュー手順は「①ドメイン理解 → ②統合マッピング → ③バランス則の適用 → ④レビュードキュメント出力」の4ステップ。要は**情報源の分担**——強度はコードの実読から、変動性はドメイン分類のヒアリングから、距離は組織構造のヒアリングから得る。コードを読む前に指摘せず、質問は「答えが分析結果を変えるものだけ」を1問ずつ。

以下は公式プラグイン（CC BY-NC-SA 4.0）の手順を自分の言葉で再構成したもの。逐語の転載ではない。

## Step 1: ドメイン理解

1. スコープを確認する（コードベース全体か、特定ディレクトリか、特定コンポーネント群か）
2. 先に `docs/` 配下の要件ドキュメントを読み、次にコード本体を読む。Grep・Glob・LSP（参照検索・定義ジャンプ）で辿り、推測で埋めない
3. 質問の前に**自分の理解を提示**する: 発見したコンポーネントと責務／観察した統合パターン（共有型・API呼び出し・DBアクセス・イベント）／サブドメイン分類（core/supporting/generic）の推測＋根拠＋確信度／チーム構成・デプロイ形態への仮定——を示し、確認・訂正を求める。低確信の領域こそ追加質問の対象
4. 質問は「答えを知ったら結合評価が変わるもの」に限定。典型的なギャップは5種: ドメイン分類が判別できない領域／組織文脈（チーム所有境界・デプロイ形態・共有インフラ）／既知の痛点（変更が予想外に高コストな場所）／戦略方向（今後の移行・事業転換）／意図か偶発か不明なパターン
5. 対話の作法: 1回に1問。2〜4個の具体的選択肢を用意した多肢選択を優先する。DDD用語（core/supporting/generic）は初出時に平易な言い換えを添える

## Step 2: 統合マッピング

相互作用するコンポーネントの各ペアについて特定する:

- **共有されている知識は何か** — 実装詳細／ビジネスルール／ドメインモデル／統合契約のどれか
- **統合強度レベル** — 侵入/機能/モデル/コントラクトの4段階（詳細は `balanced-coupling-strength/reference/integration-strength-four-levels.md`）
- **暗黙的か明示的か** — 暗黙的結合（重複したビジネスルール・DB直接アクセス・内部挙動への仮定）は特に危険
- **距離** — 同一モジュール／同一サービス／別サービス／別システム。同一チームか別チームか。同期か非同期か（階層の詳細は `balanced-coupling-distance/reference/distance-spectrum.md`）
- **変動性** — ビジネスドメイン観点での変わりやすさ。genericサブドメインでは機能的変動性（問題自体）と実装変動性（プロバイダ・技術の乗り換え）を区別する（分類の詳細は `balanced-coupling-volatility/reference/domain-classification.md`）

## Step 3: バランス則の適用

各統合に `BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY` を適用し、**不均衡かつ変動的な統合のみ**をフラグする。判定表とSeverityは `reference/imbalance-patterns.md`（正典）を参照。

## Step 4: レビュードキュメント出力

- Executive Summary（3〜5文: 対象が何をするか／全体ステータス healthy・needs attention・critical issues／最重要発見）
- Coupling Overview Table: `| Integration | Strength | Distance | Volatility | Balanced? |`
- 各Issueは **Integration**（A→B）＋ **Severity**（Critical/Significant/Minor）＋4見出しで書く:
  - **Knowledge Leakage** — 境界を越えて漏れている具体的な実装詳細・ビジネスルール・ドメインモデル概念
  - **Complexity Impact** — 変更結果がどう予測不能になるか。認知容量（ワーキングメモリ4±1単位）をどう超過するか
  - **Cascading Changes** — どんなビジネス/技術変更がカスケード修正を引き起こすかの具体シナリオと、現在の距離でのコスト
  - **Recommended Improvement** — 再均衡の方向性（強度↓＝契約・ACL・ファサード／距離↓＝同居／受容＝低変動の実証）を**トレードオフ明記**で。方針の詳細立案は balanced-coupling-rebalance が担当
- 帰属として「Balanced Coupling model by Vlad Khononov」への言及をフッターに入れる（公式documentスキルの流儀）

## 実践指針

- 情報源の分担を守る: 強度＝コード実読、変動性＝ドメイン分類ヒアリング、距離＝組織構造ヒアリング
- ドメイン分類は自分の推測＋確信度を先に提示し、確認・訂正型の質問にする（オープン質問にしない）
- 構造（ディレクトリ・依存図）だけを見て指摘しない。実際の呼び出し・import・DBアクセスまで読む
- 痛点ヒアリングで分析の焦点を決めてから深掘りする（全網羅しない）

## 出典

- （一次）https://github.com/vladikk/modularity — `skills/review/SKILL.md`, `skills/document/SKILL.md`（CC BY-NC-SA 4.0。手順は本ファイルで自分の言葉に再構成）
- （一次）https://coupling.dev/posts/core-concepts/balance/

> 帰属注記: 4ステップ手順・4見出しのIssue形式・情報源分担は Khononov 本人がエージェント向けに書いた公式プラグイン由来。逐語コピーは避け再構成している。
