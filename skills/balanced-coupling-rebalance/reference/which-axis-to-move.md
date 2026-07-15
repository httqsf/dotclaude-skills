# which-axis-to-move — どの軸を動かすかの判断

## 核心

Khononov本人の優先順位は明確: **「常に統合強度を下げる（知識共有を最小化する）ことをまず試みる。それがビジネス要件上不可能なら、距離を最小化してバランスさせる」**（InfoQ講演: "We always want to reduce integration strength; we want to always minimize the knowledge. But if... I need to use the same model of the business domain... then you have to take it into consideration, and balance it with another dimension, which is distance."）。変動性は直接操作できないが、境界の引き直しで影響範囲を制御できる。

## 判断フロー

1. **第一選択: 強度を下げる**。侵入結合→コントラクト結合へ段階的に降りる（私的インターフェース依存の排除→業務フロー知識の隠蔽→ドメインモデル共有の解消→最小契約化）。統合契約は実装詳細・機能要件・ビジネスモデルをカプセル化し、統合を明示的かつ安定にする（公式SKILL.mdの趣旨）。4段階の詳細は `balanced-coupling-strength/reference/integration-strength-four-levels.md`
2. **第二選択: 距離を縮める**。同一モデル・同一業務要件の共有が本質的に必要なら、コンポーネントを同一サービス／同一モジュール／同一チームに寄せる。「同じ物理境界の中にいる方が間違いを直すのがはるかに簡単」（InfoQ、本人発言。モジュラーモノリス文脈）
3. **距離を広げる方向の再均衡もある**: 低強度（コントラクト結合）なのに同居してモジュールが肥大化しているなら「弱×近＝低凝集の疑い」として距離を広げる候補（公式reviewスキルが明示的に検出対象とする趣旨。判定の正典は `balanced-coupling-review/reference/imbalance-patterns.md`）
4. **変動性は隔離で扱う**: 高変動な知識（coreサブドメイン）は1つの境界に閉じ込め、他モジュールへは低変動の契約だけを見せる。公式designスキルの「Change Vectors（そのモジュールだけに閉じる将来変更）」を各モジュールに定義するのがその運用形

## 各選択肢のコスト

| 選択肢 | 支払うコスト |
|---|---|
| 強度低下 | 契約設計・変換層・互換性管理のコスト。DTO乱造は逆効果（本人が警告） |
| 距離短縮 | 独立デプロイ・チーム自律性の放棄コスト |
| 何もしない | 低変動なら実害が小さく合理的（`OR NOT VOLATILITY`）。変動性の変化を監視する義務だけ残る |

## 行動原則（公式プラグインの制約条項を再構成）

- 「とにかく全部デカップリングしろ」という推奨を絶対にしない
- すべての是正案を「強度・距離・変動性のどれを・どちらへ動かすか」で根拠づける。3軸に紐づかない推奨は出さない

（いずれも公式プラグイン vladikk/modularity の制約条項の趣旨を自分の言葉で再構成したもの）

## 出典

- （一次）https://www.infoq.com/presentations/video-podcast-vlad-khononov/（本人講演。優先順位の明言）
- （一次）https://github.com/vladikk/modularity — skills/balanced-coupling/SKILL.md、skills/review/SKILL.md、skills/design/SKILL.md

> 帰属注記: 「まず強度、無理なら距離」の優先順位はInfoQ講演での本人発言（引用符内の英文は一次ソース確認済み）。「Change Vectors」は公式designスキル由来の本人の様式。変動性が直接操作不可である点は3軸モデルの帰結。制約条項2件は公式SKILL.md（CC BY-NC-SA 4.0）の逐語コピーを避けて再構成した。
