# microservice-split-decision — マイクロサービス分割前の距離判断

## 核心

分散モノリスとは「統合強度を下げずに距離だけを上げた状態」＝高強度×高距離の必然的帰結である。本人談（InfoQ）: "many companies ended up transforming their old-school monoliths into new shiny distributed monoliths. So they kind of traded local complexity into global complexity."——ローカルな複雑性をグローバルな複雑性に交換しただけ。著者はモジュラーモノリスを**マイクロサービス分解への第一歩**と明言する: "it's way easier to fix a mistake once you are in the same physical boundary."（同一物理境界内なら境界の誤りを直すのが安い）。

## 実践指針

- **判定は強度が先、距離が後**。著者の公式レビュースキルは「何でも分離せよ、とは決して推奨しない。分解は距離を増やす操作であり、増えた距離に耐えられるほど強度が既に低いか、ライフサイクル結合（同時テスト・同時デプロイ）自体が主要ボトルネックであるときにのみ分解を推奨せよ」という制約を課している（vladikk/modularity skills/review/SKILL.md の趣旨を自分の言葉で再構成）。
- 分割前チェック（著者designスキルの手順を判断基準として再構成）。候補AとBについて:
  1. **共有知識は何か** — 内部実装/業務ルール/ドメインモデル/契約のどれか（4段階の詳細は `balanced-coupling-strength/reference/integration-strength-four-levels.md`）
  2. **分割後の距離** — 別サービスか・別チームか・同期か非同期か（階層は `reference/distance-spectrum.md`）
  3. **変動性** — コアサブドメインか（分類は `balanced-coupling-volatility/reference/domain-classification.md`）
  - 高強度＋高変動のペアを高距離に置く案は「密結合」として棄却し、先に強度低減（APIによる契約の導入等）を行う。
- **生成メカニズムを理解する**: 距離を広げても共有知識（強度）はそのまま残るため、連鎖変更の確率は変わらず、1回あたりのコストだけが跳ね上がる。これが分散モノリスの正体。
- 失敗の典型（本人談・InfoQ）: "the mistake was taking those less business critical components, extracting them, and thinking that they will achieve the same result."——周辺の低変動コンポーネントでのPoC成功を、高変動のコア領域に外挿するのが罠。
- **モジュラーモノリスを先に選ぶ**: まずモジュール境界（そのレベルでの最大距離）で強度を契約結合まで下げ、境界が安定してから物理距離（サービス化）を与える。境界の誤りは同一物理境界内で直すのが安い。
- **変動性による救済**を忘れない: `BALANCE = (STRENGTH XOR DISTANCE) OR NOT VOLATILITY` により、高強度×高距離でも低変動なら実害は小さい。「動いていて変わらないレガシー統合」を最優先で剥がす必要はない。
- 書籍第10章には距離を含む連続値の数値モデルがあるが、公開ソースでスコアリング式は未確認。式を推測で書かず、二値のXOR則で判断する（著者の公式スキルも二値運用）。

## 具体例のエッセンス

- 頻繁に一緒に変わる業務フロー（注文→価格→キャンペーン→在庫）を同期APIチェーンの複数サービスに分割すると、キャンペーン仕様変更のたびに全サービスの修正と同時リリースが必要になる。物理的に分かれていても変更ライフサイクルが独立していない＝距離だけを広げた密結合。
- 逆に、距離を近づけてよいのは知識の共有が強いモジュール同士。これに反してサービス化すると、システムの大域的な複雑性が上がるだけになる（Qiita読者解説の要約・二次）。

## 出典

- （一次・本人発言）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （一次）https://github.com/vladikk/modularity （skills/review/SKILL.md・skills/design/SKILL.md。CC BY-NC-SA 4.0のため手順・判定基準は再構成）
- （二次）https://blog.sa2taka.com/post/coupling-history-and-balanced-coupling/ 、https://qiita.com/HrsUed/items/cb9a0ab075edd4c46495 ※読者解説

> 帰属注記: 「分散モノリス」という用語自体はマイクロサービス界隈で2010年代半ばから流通した既存語でKhononovのオリジナルではない。著者の貢献は、その生成メカニズムを「強度を下げずに距離だけ上げた不均衡」として3軸モデルで説明可能にした点。引用英文はInfoQ本人発言（一次確認済み）。書籍第10章の数値モデルは未取得のため式は記載しない。
