# tests-as-specification — テストは仕様書（生きたドキュメント）

## 核心
テストコードは「動かせるドキュメント / Tests as Documentation」であり、コードの仕様がひと目で分かることが重要。そのため**過度なDRYより DAMP（Descriptive And Meaningful Phrases）**を優先し、期待値はベタ書きして「そうとしか読めない」状態にする。

## 指針
- 「ワンパターン＝読みやすさ＝Tests as Documentation」。定型と可読性でテスト自体を仕様書化する。
- 期待値は計算せず**ベタ書き**（例: 誕生日1977-7-17、基準日2022-7-17 なら `45` と直書き）。テストコード側で再計算すると、テストごと間違える余地が残る（＝自作自演）。
- テストコードで過度なDRY（ヘルパ・共通化・変数化のやり過ぎ）を追うと、読み手の「脳内メモリ（認知資源）」を消費し可読性が落ちる。冗長でも一目で分かる方を選ぶ。
- テスト名は「**何を・どういう条件で・どうなるか**」を表し、新メンバーが仕様書を読まずに振る舞いを学べるようにする。
- プロダクションコードのDRYとテストコードのDAMPは別基準。テストは「読みやすさ ＞ 重複排除」。

## 良い/悪い
- 良い: `assertEquals(45, age)` のように期待値直書き＋説明的なテスト名。
- 悪い: テスト内で `expected = calcAge(birthday, today)` と本体ロジックを再実装 → 同じバグを見逃す・仕様書にならない。

## 出典・帰属
- （一次）https://www.slideshare.net/t_wada/xutp-chapter19 （Tests as Documentation）、 https://levtech.jp/media/article/interview/detail_480/
- **注記**: 「DAMP not DRY / 期待値ベタ書き / 脳内メモリ」の最も有名な発信は**伊藤淳一(jnchito)であってt-wadaではない**（方向性は整合するが出典を混同しない）。t-wada一次は「Tests as Documentation」表現。
