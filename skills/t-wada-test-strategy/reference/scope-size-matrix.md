# scope-size-matrix — スコープ × サイズの3×3マトリクス

## 核心
テストには**独立した2軸**がある——**スコープ（Unit / Integration / E2E＝テスト対象の広さ）**と**サイズ（S / M / L＝リソースアクセスの範囲）**。この2軸で **3×3マトリクス**を作り、**対角線付近（Unit×Small、Integration×Medium、E2E×Large）が「コスパの良い」推奨領域**になる。スコープとサイズは混同されがちだが別物。

## 指針
- 自分のテストを「スコープ何／サイズ何」の**両方でラベリング**して現状把握する。
- **理想は「サイズを上げずにスコープを広げる」**——複数コンポーネントを統合しても Small のまま保つ（`small × 統合`）。
- 反面教師は「**Large なのに Unit**」——対象は狭いのに実物依存で遅く・flaky。この象限を作らない。
- ドメイン層は `small × 単体`、Controller/Service は「**small だが統合**」を狙い、大半を Small に寄せてパイプラインのスループットを上げる。
- 再現テスト（バグ再現）は `small × 単体`が最適形。
- DBを使うと size が small→medium に上がる。だからこそ Repository 層より内側（ドメイン）を Small に保つ設計が効く。

## 出典
- （一次）https://speakerdeck.com/twada/automated-test-knowledge-from-savanna-202305-edition
- （二次・本人議論）https://swet.dena.com/entry/2023/11/13/170000 、 https://logmi.jp/main/technology/330972
