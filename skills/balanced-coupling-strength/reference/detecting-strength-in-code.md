# detecting-strength-in-code — コードレビューでの統合強度判定

## 核心

統合強度の判定は「境界を越えて共有されている知識の種類」を**実コードから読み取る**作業であり、ディレクトリ構成や依存図だけでは判定できない。公式reviewスキルの原則（趣旨）: "**Never identify issues from structure alone**"——関数呼び出し・import・共有データ構造・DBアクセスパターンの実物を調べよ。特に機能結合の一部（重複実装＝wireless coupling）は静的解析ツールやアーキテクチャ図では発見できず、業務知識を踏まえた読解が必須。

## レベル別の検出チェックリスト

**侵入結合の兆候**（公開インターフェース以外での統合）:
- 他コンポーネント/他サービスが所有するDBテーブルへの直接SELECT/UPDATE（例: 注文サービスが `payment_transactions` を直接更新）
- リフレクション・モンキーパッチでのprivateメンバーアクセス
- 文書化されていない・非公開のAPIエンドポイントやファイル形式への依存
- 「相手の作者がこの統合の存在を知っているか？」に No なら最悪度

**機能結合の兆候**（ビジネス要件の知識共有）:
- 同じビジネスルールの重複実装（フロント/バックのバリデーション、複数サービスの割引計算）。コード上のリンクが**ない**のに仕様変更で必ず両方直すファイル群
- 呼び出し側が業務フローの**順序**を知っている（reserveStock→authorizePayment→createShipment の並びと失敗時補償を呼び出し側が管理）
- 複数操作にまたがる同時実行制御・トランザクション整合の知識共有（動的コナーセンスの値・同一性）
- タイムアウト値・実行タイミングへの暗黙依存（動的コナーセンスのタイミング）
- git履歴で「一緒に変更されるが依存関係のないコンポーネント」はwireless couplingの候補（共変更分析の正しい用途は `balanced-coupling-review/reference/evidence-gathering.md`）

**モデル結合の兆候**（ドメインモデルの共有）:
- 内部エンティティ/ORMモデルをそのままAPIレスポンス・イベントペイロード・共有ライブラリで公開（DBスキーマがフロントまで漏れる `user_status_code: 3` 等）
- 利用側が受け取った構造の一部しか使っていないのに全体に依存（スタンプ結合的兆候）
- 複数サービスから参照される共通ライブラリ内のドメインモデル群（shared/User, Order, Payment…）
- 「ドメインの洞察でモデルを改良したいとき、参照している全コンポーネントの同時変更が必要になるか」で判定

**コントラクト結合の確認**（最弱だが要検証）:
- Facade・OHS/公表された言語・ACL・DTOなど統合専用の契約が存在する
- ただし**契約が本当に知識を隠しているか**を必ず確認する。内部モデルと属性が一対一のDTOは見かけだけのコントラクト結合（`reference/contract-coupling-pitfalls.md`）
- 静的コナーセンスで度合いを見る: 位置依存のペイロード（配列添字に意味）＞マジックナンバー共有（意味）＞名前・型のみ、の順で弱い方へ

## 実践指針

- レビューの3問セット（本人推奨）: (1) **何の知識が共有されているか**（＝強度レベル特定）、(2) **物理的距離はどれか**（同一ファイル〜別システム）、(3) **どれくらい変動するか**。強度単独で指摘を書かない。
- 「強い×遠い」を最優先で洗う: 別サービス・別チーム間の侵入/機能/モデル結合が最危険。同一クラス内の強結合は緊急度が低いことが多い。
- 重複実装の検出は仕様側から: 「この仕様が変わったら直すファイルはどれか」を業務の観点から洗い出す。コード検索やアーキ図では出てこない。
- 「隠蔽している知識は何か」をモジュールごとに言語化する。書けないモジュールはただのフォルダ分割。
- 低変動の指摘は下位に置いてよい: 変わらないレガシーへの侵入結合は指摘リストの下位（本人が実用主義として許容。判断は `balanced-coupling-core/reference/balance-formula.md` へ）。

## 出典

- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/review/SKILL.md （"Never identify issues from structure alone"、共有知識の問い）
- （一次）https://raw.githubusercontent.com/vladikk/modularity/main/skills/balanced-coupling/SKILL.md （レベル別シグナル）
- （一次・本人インタビュー）https://techleadjournal.dev/episodes/188/ 、https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://zenn.dev/praha/articles/0fef0bb54d528d ※二次。重複実装は「静的解析ツールやアーキテクチャ図では発見できず」

> 帰属注記: レビュー手順・判定基準はvladikk/modularity公式プラグイン（CC BY-NC-SA 4.0）の趣旨を自分の言葉で再構成したもの（逐語コピーではない）。wireless couplingはKhononov本人の口語表現（Tech Lead Journal）。書籍第10章に数値モデル（0-10スケール）があるが公開ソースで式は未確認——二値XOR則で判断せよ（著者の公式スキルも二値運用）。
