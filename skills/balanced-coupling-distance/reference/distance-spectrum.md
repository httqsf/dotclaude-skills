# distance-spectrum — 距離の階層（本ファイルが正典）

balanced-coupling-* ファミリーにおける距離の階層の**正典**。他スキルで階層に触れる場合は本ファイルを参照する。

## 核心

距離はスペクトラムを成し、遠くなるほど同時変更の認知的努力と調整コストが単調に増える。著者が一次資料で明示する階層は次の**5段階**である。

> **methods → objects → namespaces/packages → (micro)services → systems**

"Distance increases through levels of abstraction: methods -> objects -> namespaces/packages -> (micro)services -> systems. At each level, the cognitive effort and coordination cost of simultaneous change increases."（coupling.dev / vladikk/modularity）

これに**所有権（ownership）の軸**が重なる: 同一人物 → 同一チーム → 同一部門 → 同一組織（→ 別組織）。"the greater the ownership distance (whether components are maintained by the same person, team, department, or organization), the higher the coordination effort."（olano.dev の書籍要約より・二次）

## 実践指針

- 距離の評価は「技術的階層のどこか」×「所有権階層のどこか」の2軸で行う。同じサービス分割でも、所有チームが同じか別かで距離は変わる（詳細は `reference/sociotechnical-distance.md`）。
- 各段階で加算されるコストの目安（著者の5段階＋所有権軸と本人のInfoQ発言に基づく実務用の展開であり、書籍の逐語ではない）:
  - **methods / objects（同一ファイル・クラス圏）**: 1人が1エディタ・1コミットで完結。コストはほぼ認知負荷のみ。
  - **namespaces/packages**: 公開インターフェースの変更影響、依存方向の管理が加わる。
  - **(micro)services**: API契約の調整、後方互換性、バージョニング、デプロイ順序、障害連鎖の考慮が加わる。別チーム所有なら複数PR・複数レビュー・優先度調整も加わる。
  - **systems / 別組織**: 契約・SLA・交渉。変更を「依頼」しかできない（自分では変更不可能）というコストが加わる。
- 距離を広げることで得られる利得を必ず併記する。"many 'monolith to microservices' refactorings started with the goal of reducing lifecycle dependencies."（coupling.dev）——距離増加は独立テスト・独立デプロイという便益との引き換えである。
- 階層は固定の物差しではなく**フラクタルに再帰**する。モジュール内を設計しているならモジュール間境界がそのコンテキストの最大距離。マイクロサービスが存在しなくても距離の議論は成立する（フラクタル性の全体像は `balanced-coupling-core/reference/fractal-geometry.md`）。
- 読者や解説記事による6段階・10段階などの細分化リストが流通しているが、著者一次の階層はこの5段階＋所有権軸である。細分化リストを正典として引用しない。

## 具体例・コード例

同じ知識（税率計算のルール）を共有するとき、置く距離で連鎖変更のコストが変わる（TypeScript。他言語でも同等の考え方）:

```ts
// 距離: methods/objects — 同一ファイル内。変更は1コミットで完結
function taxRate(region: Region): number { /* ... */ }
function totalPrice(items: Item[], region: Region): number {
  return subtotal(items) * (1 + taxRate(region));
}

// 距離: (micro)services — 税率ルールが別サービスにも実装されていると、
// 税制変更のたびにAPI契約調整・後方互換・同時リリース調整が発生する
const rate = await fetch("https://tax-service/rates?region=" + region);
```

強度（共有知識の種類）が同じでも、距離が上がるほど1回の連鎖変更のコストが跳ね上がる。だから均衡則は「強い結合は近くに、遠くに置くなら知識共有を減らす」となる。

## 出典

- （一次）https://coupling.dev/posts/dimensions-of-coupling/distance/
- （一次）https://github.com/vladikk/modularity （skills/balanced-coupling/SKILL.md §2.2・§3）
- （一次・本人発言）https://www.infoq.com/presentations/video-podcast-vlad-khononov/
- （二次）https://olano.dev/blog/balancing-coupling/ ※所有権軸の出典。二次であることを明記

> 帰属注記: 5段階の技術的階層はKhononov一次資料の逐語に基づく。所有権軸（person/team/department/organization）は書籍要約（olano.dev）経由で、逐語の断定は避ける。「各段階で加算されるコスト」の内訳は本スキル調査時の再構成であり、著者の逐語として引用しない。
