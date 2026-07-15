# integration-strength-four-levels — 統合強度の4段階（正典）

## 核心

統合強度は「コンポーネント境界を越えて共有されうる知識の**4つの基本タイプ**」＝侵入/機能/モデル/コントラクト結合を、あらゆる抽象度（変数〜サービス〜システム）に適用するKhononovオリジナルの分類。本人の言葉（趣旨）: "The four terms are contract, model, functional, and intrusive coupling. They are not defining the amount of knowledge. They're defining the **type** of knowledge."（Tech Lead Journal #188）。「知識を『測る』のではなく『分類する』方が実践的」（coupling.dev）。

設計原理: 共有知識が多いほど、共有部分が変わったときの連鎖変更の確率が上がる。ただし強度は単独で善悪を判定せず、距離・変動性と組み合わせて評価する（`balanced-coupling-core/reference/balance-formula.md`）。

## 統合構造（何をどう束ねたモデルか）

- **古典モジュール結合からの写像**: 「何を共有しているか（知識の種類）」の枠組みを継承し、粒度の不揃いと時代がかった用語を理由に4レベルへ再編。対応は **内容→侵入、共通/外部/制御→機能、スタンプ→モデル、データ→コントラクト**（詳細は `reference/classic-module-coupling.md`）。
- **コナーセンスの吸収**: レベル間比較＝4段階、**レベル内比較＝コナーセンス**。静的コナーセンスでコントラクト/モデル結合内の度合いを、動的コナーセンスで機能結合内の度合いを比較する（詳細は `reference/connascence.md`）。
- **両モデルの盲点の補完**: 物理的に接続されていないコンポーネント間の重複実装（wireless coupling）を機能結合として捕捉。
- **暗黙的↔明示的のスペクトラム**: もう一つの軸。コントラクト結合が最も明示的、侵入結合が最も脆く暗黙的。機能結合も暗黙的になりうる。

## 4レベル（強い順。コード例は説明用の創作例、他言語でも同等の考え方）

### (1) 侵入結合（Intrusive Coupling）— 最強

**私的（プライベート）なインターフェース**が統合に使われる状態。適切な統合インターフェースの代わりに、プライベートオブジェクト、内部データベース、その他の実装詳細を使う（coupling.dev趣旨）。共有される知識は「**実装に関するすべての知識**」と仮定しなければならない——テーブル名・カラム名・状態値・状態遷移・トランザクション規則など全部。脆く（fragile）かつ暗黙的。侵入された側の作者が統合の存在自体を知らない場合、リスクはさらに増す。"any change can potentially break the integration."（InfoQ趣旨）

```ts
// 注文サービスが決済サービスのDBを直接更新（侵入結合）
await database.execute(`
  UPDATE payment_transactions SET status = 'refunded' WHERE id = ?
`);
```

### (2) 機能結合（Functional Coupling）

複数のコンポーネントが**機能要件の知識**を共有する状態。焦点を「どう実装されているか」から「**何を**実装しているか」へ移す。共有知識はビジネスルール、業務フローの順序、同時実行制御、補償処理。極端な形は**重複した知識**——同じビジネスルールがフロントとバックの両方に実装され、仕様変更が両方に同時反映されないと不整合に陥る。

**wireless coupling（本人の口語表現）**: 「重複したビジネスロジックは、明示的には結合していないコンポーネントに存在しうるが、それでも一緒に変わる必要がある」——**接続のない機能結合は静的解析やアーキテクチャ図では発見できない**（検出は `reference/detecting-strength-in-code.md`）。

度合いは動的コナーセンス（実行順序・タイミング・値・同一性）で比較する。書籍紹介文では「トランザクション的・時間的度合い」への細分化に言及（Pearson。書籍内の正確な用語は目安として提示）。

```ts
// 呼び出し側が業務フロー全体（順序・補償）を知っている（機能結合）
await reserveStock();
await authorizePayment();
await createShipment();
await updateOrderStatus();
```

### (3) モデル結合（Model Coupling）

コンポーネントが**ドメインモデルの知識**（エンティティ定義、値オブジェクト、データモデルの構造と意味論）を共有する状態。本人談（趣旨）: 「同じモデルに基づく2つのコンポーネントがあるなら、ビジネスドメインへの洞察でモデルを改良したくなったとき、両方を同時に変えることになる」（InfoQ）。ビジネスドメインの全知識はコード化できないため、目的に関連する知識だけを捉えるモデルを作る——DDDが「一つの巨大モデルではなく問題に特化した複数のモデル」を推すのはこのため。機能結合より明示的。

```ts
// 配送サービスがCustomer全体を受け取る＝請求・与信知識まで共有（モデル結合）
type Customer = {
  id: string;
  rank: "NORMAL" | "GOLD";
  billingAddress: Address;
  creditLimit: number;
  internalScore: number;
};
```

### (4) コントラクト結合（Contract Coupling）— 最弱

共有知識の最も低いレベル。統合コントラクトの目的は、実装詳細・機能要件・ビジネスモデルに関する**すべての知識をカプセル化**し、統合を明示的かつ安定にすること。本人談（趣旨）: コントラクトとは "**a model of a model**"——実装モデルの知識を境界の外に一切漏らさない（InfoQ）。判定基準は「モデルが変わっても、その変更を同じ統合コントラクトの背後に**封じ込められる**か」（Tech Lead Journal趣旨）。実現パターン: Facade、オープンホストサービス/公表された言語、腐敗防止層、DTO。度合いは静的コナーセンスで比較。**ただしDTOを挟むだけでは達成できない**（`reference/contract-coupling-pitfalls.md`）。

```ts
// 配送に必要な知識だけの統合専用契約（コントラクト結合）
type ShippingDestination = {
  postalCode: string;
  address: string;
  recipientName: string;
};
```

## 実践指針

- 判定の第一問は常に「**この境界を越えて共有されている知識は何か**——実装詳細か、ビジネスルールか、ドメインモデルか、統合契約か」。
- 量ではなく種類で分類する。強度は単独で善悪判定しない——距離・変動性と必ず同時評価（「1次元だけで結合を評価してはならない」）。
- 高変動域では: 強い知識共有（機能/モデル結合）は近くに置く＝高凝集。遠いもの（別サービス・別チーム）はコントラクトで統合＝疎結合。
- 低変動なら実利的ショートカットも許容——「変わらないアップストリームへの侵入結合は、変動性が低ければ実用上許容できる」（本人がレガシー統合の文脈で明言）。可否の判断は `balanced-coupling-core/reference/balance-formula.md` のBALANCE式へ委譲。
- バウンデッドコンテキストを越える統合は必ずコントラクトに（OHS・公表された言語・ACL）。

## 出典

- （一次）https://coupling.dev/posts/dimensions-of-coupling/integration-strength/ （定義）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md （古典対応・度合い・パターン）
- （一次・本人インタビュー）https://techleadjournal.dev/episodes/188/ （"type of knowledge"、"model of a model"、wireless coupling）
- （一次・本人インタビュー）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （公式書籍紹介）https://www.pearson.com/en-us/subject-catalog/p/balancing-coupling-in-software-design-successful-software-architecture-in-general-and-distributed-systems/P200000000372/9780137353576

> 帰属注記: 4段階（侵入/機能/モデル/コントラクト）はKhononovオリジナル（原著2024年）。部品は古典結合度（Stevens/Myers/Constantine, 1974）とコナーセンス（Page-Jones, 1992）——この三層の帰属を崩さない。訳語「侵入結合/機能結合/モデル結合/コントラクト結合」は邦訳（島田浩二訳、インプレス2025）由来と推定。「契約結合」の表記揺れがあるが本スキルでは「コントラクト結合」に統一。引用符付き英文はWeb記事・要約経由であり一語一句の逐語保証はない（趣旨として扱う）。コード例は書籍掲載例ではなく説明用の創作例。
