# building-test-culture — テスト文化を組織に根付かせる

## 核心
自動テストは「書く時間がないから省く」ものではなく、「**書かないから時間がなくなる**」もの。テストのないコードはレガシー化し保守に時間を奪われるため、投資は短期間で回収される（損益分岐点の目安：内部品質への投資は約1ヶ月、テスト自動化は約4回の実行＝「質とスピード」。講演版によっては「6ヶ月以内に効果」という保守的表現もあるが、資料化の際は `t-wada-quality-mindset/reference/quality-and-speed.md` の数値と揃えること）。ただしテスト自体が品質を上げるのではなく、品質を支えるのは「設計とプログラミング」。文化構築は一気にではなく、成功事例を積み重ねて少しずつ広げる「**陣取り合戦**」で進める。

## 指針
- 最初から全部やろうとしない／TDDに執着しない。小さく始めて成功体験を積む（陣取り合戦）。
- 対象は「**リスクの高さ × 手動テストコストの高さ**」で優先順位づけし、高リスク×高コスト領域に集中する。
- テストは外部委託せず**開発チーム自身が所有**する（外注すると保守力が育たず「アイスクリームコーン」型に陥る）。
- チームに広げるときは押し付けない。技術論ではなく「**損得・苦楽の話**」に変換して伝える。
- 経営層へはROI（内部品質への投資は約1ヶ月・テスト自動化は約4回の実行で回収。不具合の発見が遅れるほどコスト増）とビジネス成果への波及で説得する。数値は `t-wada-quality-mindset/reference/quality-and-speed.md` を正典とする。
- 「社会的変化は自分自身から始まる」。まず自分が書けるようになり、チームへ波及させる。

## 出典
- （一次）https://speakerdeck.com/twada/strategy-and-tactics-of-building-automated-testing-culture-into-organization-2020-autumn-edition
- （一次）https://speakerdeck.com/twada/building-automated-test-culture-2022-autumn-edition
- （一次インタビュー）https://levtech.jp/media/article/interview/detail_480/ 、 https://findy-code.io/engineer-lab/t-wada

> 帰属: 「テストを書く時間がない、ではなく書かないから時間がなくなる」はJames Grenning（Agile Japan基調講演）の言葉としてt-wadaが引用。
