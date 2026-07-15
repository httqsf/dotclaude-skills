# sociotechnical-distance — 社会技術的距離（チーム境界が実効距離を決める）

## 核心

距離は純粋に技術的な量ではない。**コード構造がまったく同じでも、所有チームが分かれていれば実効距離（effective distance）は大きくなる**——著者は公式レビュースキルでこれを制約として明文化している（vladikk/modularity skills/review/SKILL.md の趣旨を再構成。逐語引用は避ける）。凝集もこの距離の言葉で再定義される: "high cohesion occurs when components that share considerable knowledge are located at low distance, making co-evolution cheap."（coupling.dev）。

## 実践指針

- 距離評価では必ず「関与チーム数」を確認する。本人発言（InfoQ）: "we have social technical factors, those two components are implemented by the same team, or do we have to coordinate the change with multiple teams? And suddenly the distance can grow even larger."
- 同一構造の2マイクロサービスでも、同一チームが両方を持つ場合と別チームが1つずつ持つ場合では距離が異なる。後者ではAPI変更のたびに他チームとの合意形成・優先度調整・リリース調整が必要になる。
- チーム分割は、コードを1行も動かさなくても距離を増やす**設計変更**である。逆にチーム統合（所有権の一本化）は距離を縮める打ち手になる。組織変更の提案・評価では、対応する結合バランスの変化を必ず点検する。
- ただしチーム内に近く置くとライフサイクル結合が強まる（同時テスト・同時デプロイ）。coupling.dev は「同一チーム実装は協調オーバーヘッドを減らすがデプロイ結合を増やす」トレードオフとして描く。**距離設計＝チーム設計**であり、片方だけでは決められない。
- レビュー時はコードに加えて組織情報（チーム所有権の境界・デプロイトポロジー・共有インフラ）を入力として要求する。コード構造だけでは実効距離は読み取れない（手順は `reference/distance-in-review.md`）。

## 具体例のエッセンス

- 強度×距離の4象限（著者プラグイン §4.1 の再構成）: 低強度×低距離＝低凝集（無関係なものの同居、big ball of mud へ漂流）／高強度×低距離＝高凝集／低強度×高距離＝疎結合／高強度×高距離＝密結合（分散モノリスへの一歩）。疎結合と高凝集はどちらも「バランスした結合」の2形態であり、別々の美徳ではない。変動性を含めた不均衡判定表は `balanced-coupling-review/reference/imbalance-patterns.md`（正典）を参照。
- 「一緒に変わるべきもの（知識共有が多いもの）を低距離に置く」——これが凝集の操作的定義。

## 出典

- （一次）https://github.com/vladikk/modularity （skills/balanced-coupling/SKILL.md "Distance Is Socio-Technical" 節・skills/review/SKILL.md。CC BY-NC-SA 4.0のため判定基準は再構成）
- （一次）https://coupling.dev/ および /posts/dimensions-of-coupling/distance/
- （一次・本人発言）https://www.infoq.com/presentations/video-podcast-vlad-khononov/

> 帰属注記: 距離の社会技術性（チーム境界が実効距離を変える）はKhononovの主張。コンウェイの法則（組織構造がアーキテクチャを形づくる）は Melvin Conway (1968) の既存概念であり、著者はプラグイン内でこれに接続している（InfoQ登壇の本人発言では名指しせず現象として説明）。「著者オリジナルの軸」と「既存概念への接続」を混同しない。
