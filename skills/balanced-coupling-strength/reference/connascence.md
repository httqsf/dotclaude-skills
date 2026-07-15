# connascence — コナーセンス（レベル内の度合いの物差し）

## 核心

コナーセンスはMeilir Page-Jonesが1990年代にOOP普及を受けて提唱した結合モデルで、「一方の変更が他方の変更（または確認）を要求する関係」を静的/動的の約9種で分類する（語源はラテン語「共に生まれた」）。Khononovはこれを古典モジュール結合より細粒度と評価し、統合強度モデルでは**レベル間の比較ではなく、同一レベル内の度合い（degree）を測る物差し**として組み込んだ。

Khononovが引く定義（趣旨）: "Two components are said to be born together if a change in one necessitates a corresponding change in another, or if a reasonable change can be postulated that would affect both components."（coupling.dev）

## 分類（coupling.devでのKhononov自身の整理）

**静的コナーセンス（コンパイル時、弱い順）**:
1. **名前（Name）**: 変数名・メソッド名などの名前の一致。
2. **型（Type）**: 型（クラス/プリミティブ/シグネチャ）の一致。
3. **意味（Meaning）**: 値に特別な意味を割り当てる（例: `statusId = 7` の意味の共有）。
4. **アルゴリズム（Algorithm）**: 同じアルゴリズムへの合意（例: 暗号化プロトコル）。
5. **位置（Position）**: 位置に意味を割り当てる。静的の最強。コンパイラで検出できない誤りを生む（例: `SendEmail(string[] values)` の添字に意味）。

**動的コナーセンス（実行時。どれも静的のどれよりも強い、弱い順）**:
1. **実行順序（Execution）**: 操作が特定の順序で起きる必要がある（例: open→commit/rollback）。
2. **タイミング（Timing）**: 実行間の明示的な時間制約（例: 60秒でタイムアウトするDB接続）。
3. **値（Value）**: 複数の値がトランザクショナルに一緒に変わる必要がある（例: 三角形の3辺）。
4. **同一性（Identity）**: 2つのコンポーネントが第三の**同一インスタンス**への参照を要する。全体の最強（例: 共有メモリ、共有DB、unit of work）。

## 実践指針

- リファクタリングの基本方針は「強いコナーセンスを弱いものに置き換える」。代表例: **位置→名前**（`createUser("Suzuki", 35, true)` → `createUser({name, age, sendWelcomeMail})`）。
- 「意味のコナーセンス」（マジックナンバー・ステータスコードの意味共有）は名前や型（enum、値オブジェクト）に置き換える。
- 動的コナーセンスは静的より常に強い。実行時にしか壊れない依存（順序・タイミング・値・同一性）を見つけたら優先的に弱める。
- 局所性とセットで判断する: 同一関数内の位置コナーセンスは許容できても、サービス境界を越えた位置・意味の共有は危険（距離軸の先取り。詳細は `balanced-coupling-distance/reference/distance-spectrum.md`）。
- Khononov流の使い方: **静的コナーセンスでコントラクト結合・モデル結合内の度合いを、動的コナーセンスで機能結合内の度合いを比較する**。レベルまたぎの比較には使わない。

## コード例（説明用の創作例。他言語でも同等の考え方）

```ts
// 悪い例: 位置のコナーセンス — 添字に意味がある
createUser("Suzuki", 35, true);

// 良い例: 名前のコナーセンスへ弱める
createUser({ name: "Suzuki", age: 35, sendWelcomeMail: true });
```

## 出典

- （一次）https://coupling.dev/posts/related-topics/connascence/ （分類・例・統合強度への組み込み）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md （度合いとしての利用法）
- （二次）https://blog.sa2taka.com/post/coupling-history-and-balanced-coupling/ ※二次。Page-Jones 1992、評価3軸

> 帰属注記: コナーセンスはMeilir Page-Jones由来（初出は1992年のACM論文 "Comparing Techniques by Means of Encapsulation and Connascence" とされる）。「9種類」という数え方や「強さ/局所性/程度」の評価3軸の正確な出典表現は文献間で揺れがあり断定しない（coupling.devでは静的/動的階層が中心）。「レベル内の度合いの物差しとしての統合」はKhononovオリジナル。
