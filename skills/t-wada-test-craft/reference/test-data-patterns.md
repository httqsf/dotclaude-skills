# test-data-patterns — テストデータ準備パターン

## 核心
複雑なテストデータ準備を読みやすく・テスト間で独立に保つためのパターン群。**Object Mother**＝テスト用オブジェクトを生成するファクトリ（「カタログ」）、**Test Data Builder**＝Builderパターンでデータを組み立てる（「工場」）。両者は補完関係。パラメタライズドテストは同一ロジックを入力違いで反復検証する手法。

## 指針
- **Object Mother**: 典型的な「ペルソナ」を一箇所に集約し再利用。命名で可読性が上がる。難点は**共有により変更が他テストに波及**（Mother変更で無関係テストが壊れる）。
- **Test Data Builder**: 「デフォルト値＋変えたい部分だけ上書き」で組み立て、テスト間依存を防ぐ。オブジェクト依存が複雑・多数生成時に有効。
- 使い分け: 定型シナリオ・少数パターン→Object Mother、複雑/バリエーション豊富→Builder。両者併用（MotherがBuilderを呼ぶ）も可。
- **パラメタライズドテスト**: 各言語フレームワークの機能で「テスト毎に違う部分だけを書く」。ただし「どのケースで落ちたか分かる」ように（1テスト1アサーション原則との両立に注意）。

## 良い/悪い
- 良い: `aUser().withAge(45).build()` のように意図が読めるBuilder。
- 悪い: 各テストで巨大なオブジェクトをフルにnewして本題が埋もれる／Object Motherを無節操に共有し密結合。

## 出典・帰属
- （一次周辺）https://t-wada.hatenablog.jp/entry/design-for-testability （パラメタライズドテスト言及）
- **注記**: t-wadaの一次ソースは薄い。Test Data Builder / Object Mother の概念的出典は **Nat Pryce（Builder）・Martin Fowler（Object Mother）**。「一般的パターンであり、t-wada一次発信は限定的」と理解する。
