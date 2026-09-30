---
title: 広聴AI
type: concept
created: 2026-05-16
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2025-08-07__日記2025-08-07.md
  - raw/scrapbox_kozaneba/2025-08-10__週記2025-08-10~2025-08-20.md
  - raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md
  - raw/scrapbox_kozaneba/2025-09-03__日記2025-09-03.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md
  - raw/scrapbox_kozaneba/2025-09-14__週記2025-09-14~2025-09-22.md
  - raw/scrapbox_kozaneba/2025-10-07__日記2025-10-07.md
---

## 定義

**広聴AI** は、[ブロードリスニング](ブロードリスニング.md) を AI で支援する具体的な実装/プロジェクト名。DD2030（デジタル民主主義2030）が開発する OSS で、2025 年に nishio が手がけており、**大量の意見テキストを embedding して 2 次元に並べ、クラスタリングして可視化する** 仕組みを持つ。

[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md):

> 広聴AI は embedding して UMAP で二次元にしてから凝集クラスタリングしている。

## Kozaneba との関係(2つの Kozaneba 議論)

2025 年に **広聴AI と Kozaneba の関係**(あるいは融合可能性)が浮上している。

[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md):

> 広聴AI は embedding して UMAP で二次元にしてから凝集クラスタリングしている。
> 待てよ? 最終的に **付箋配置に落とすなら次元削減で 2 次元に落とす必要はないのかも?**
> というか実質的にグラフベースの次元削減とやってることがほぼ同じになりそう。

[pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md):

> 広聴AI 的なもののリーフノードを単なる点ではなく **付箋にすること** を考えてる。

つまり、広聴AI のクラスタ可視化と Kozaneba のこざね配置は本質的に同じ問題に二つのアプローチで挑んでいる、という気づき。これが「**2つの Kozaneba**」議論につながっている(詳細は [Canvas移行の検討](../themes/Canvas移行の検討.md))。

## 実装上の話題

- **散布図の改善**: 雑に [Devin](../entities/Devin.md) に指示したらコケた([日記2025-08-07](../../raw/scrapbox_kozaneba/2025-08-07__日記2025-08-07.md))
- **アスペクト比、ラベルを引き出し線にする実験**([週記2025-08-10~2025-08-20](../../raw/scrapbox_kozaneba/2025-08-10__週記2025-08-10~2025-08-20.md))
- **濃いクラスタ表示**: リストコントロールから選んだものにハイライトと引き出し線が出る UI([日記2025-09-03](../../raw/scrapbox_kozaneba/2025-09-03__日記2025-09-03.md))
- **YouTube コメント広聴AI**: Azure にデプロイ([週記2025-09-14~2025-09-22](../../raw/scrapbox_kozaneba/2025-09-14__週記2025-09-14~2025-09-22.md))
- **データ実験**: チームみらいの問題意識抽出データを使いたい([日記2025-10-07](../../raw/scrapbox_kozaneba/2025-10-07__日記2025-10-07.md))

## サイド実験としての Canvas プロトタイプ(2025-08)

2025-08-26~27 に nishio + GPT-5/Claude が、広聴AI のデータ(意見テキストを embedding + UMAP)を **Kozaneba 風付箋として 1 万件描く Canvas プロトタイプ** を作った([pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)、repo: `nishio/canvas_kozaneba_prototype`、deploy: `canvas-kozaneba-prototype.vercel.app`、local: `work/canvas_kozaneba_prototype/`)。

これは **Kozaneba 本流(`work/kozaneba/`)の改修ではなく独立した別実装** で、Plan A / Plan B のどちらにも属していない **第三のサイド実験**。広聴AI 側で「リーフノード = 単なる点 ではなく付箋」を試したいというニーズが動機。

### 2025-08-29 時点の到達点と未解決の問い(2026-06-03 コード + Scrapbox 再確認)

**default UX(`/`)に出ているもの**: `StickyNotesZoomDemo` — 1 万件付箋を LOD でズーム表示するだけのデモ。クラスタ / AI 表札なし。

**experimental サブルート `#/clusters` のみに存在するもの**(default 動線なし):

- **配置**: `NOTE_SIZE=120`、`POSITION_SCALE=4000` で格子スナップ + 半径 0..20 螺旋探索
- **density-driven cluster**: 4 近傍連結成分でクラスタを抽出、`min-size=10` でフィルタ、矩形オーバーラップは併合 + 正方形化を収束まで反復(commit `e542fc7`)
- **semantic zoom**: 個別付箋の見かけ幅が画面上 80px 以上か未満かで「個別表示」「クラスタ大きな付箋表示」を切替(`StickyNotesClustersView.tsx:304`)
- **AI 自動表札**: `summarizer.ts` で `/api/summarize` 呼び出し、OpenRouter 経由なら `openai/gpt-4o-mini`(`max-tokens=600`)で precompute、`clusters_summary.json` として静的配信
- **first-class frame**: `ClusterSummary { id, rect, noteIds, texts, summary? }` 構造

**最終 commit 日(2025-08-29)に nishio が未解決のまま残した問い**([pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)):

- **クラスタ抽出方針が決まっていない**: 連結成分(オーバーラップ発生)vs 10 マス割り(自然なクラスタが分割)。どちらも欠点があり結論なし
- **マージ閾値が定まっていない**: 3 割で実験、2 割にすると連鎖マージが起きる。最後の一文は「**たぶんこのあたりが連鎖的に繋がってしまうのだと思う**」で終わる
- **そもそも路線統合すべきかに疑問**:「**やっぱ 10000 件以上のものをどうするかという路線と、1000 件未満の付箋を作りながら構造化していく Kozaneba は無闇に同一視しない方が良い気がするなぁ**」
- 翌日以降の commit なし

つまりこのプロトタイプは **「設計判断の途中で停止した探索」** であって、「サーベイ結論を先取りした完成品」ではない。[畳むUIの再設計](../themes/畳むUIの再設計.md) の 2026-06 結論に対応する rendering ロジックは draft として存在するが、未解決の設計判断を本流側でも別途解く必要がある。

### 2026-06-03: クラスタ抽出方針の着地方向 — [Google Maps メタファ](../themes/Google_Mapsメタファ.md)

連結成分 / 10 マス割り の二者択一に対し、nishio が **「Google Maps のメタファで開き直る、タイル方式でいい」** という第三の道を出した。分割を許容する代わりに、マージ閾値不要・予測可能・ユーザ認知コスト最小、を得る。「正しいクラスタ」を諦めて「タイルだから」というメタファで意味の欠如を正当化する解法。これによりクラスタ抽出方針 / マージ閾値の 2 つは解消方向、路線統合判断と default UX 統合の 2 つは未解決のまま。詳しくは [Google Maps メタファ](../themes/Google_Mapsメタファ.md)。

詳細は [Canvas 1 万件デモの拡張](../themes/Canvas_1万件デモの拡張.md) と [人間が動かすから隙間ができる](../themes/人間が動かすから隙間ができる.md)。

### このプロトタイプから抽出した一般概念

- [密度の高さを大きさに変換して可視化](密度の高さを大きさに変換して可視化.md) — 中心可視化原則
- [認知メタファのデザイン](../themes/認知メタファのデザイン.md) — KDE ではなく付箋を選んだ根拠
- [人間が動かすから隙間ができる](../themes/人間が動かすから隙間ができる.md) — 本流(人間駆動)とプロトタイプ(アルゴ駆動)の本質的な違い
- [内部構造がわかりやすい](内部構造がわかりやすい.md) — 機能 vs 見栄えのトレードオフ

詳細は [Canvas 1 万件デモの拡張](../themes/Canvas_1万件デモの拡張.md)。

## 関連

- [ブロードリスニング](ブロードリスニング.md) — 親概念
- [Canvas移行の検討](../themes/Canvas移行の検討.md) — 「2つの Kozaneba」議論、プロトタイプ本体
- [Canvas 1 万件デモの拡張](../themes/Canvas_1万件デモの拡張.md) — プロトタイプの拡張案
- [人間が動かすから隙間ができる](../themes/人間が動かすから隙間ができる.md) — 本流と広聴AI 路線の構造的な違い
- [認知メタファのデザイン](../themes/認知メタファのデザイン.md) — 可視化選択の哲学
- [Plurality](Plurality.md) / [Audrey Tang](../entities/Audrey_Tang.md)
- [Kozaneba](../entities/Kozaneba.md)

## Sources

- [日記2025-08-07](../../raw/scrapbox_kozaneba/2025-08-07__日記2025-08-07.md)
- [週記2025-08-10~2025-08-20](../../raw/scrapbox_kozaneba/2025-08-10__週記2025-08-10~2025-08-20.md)
- [pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)
- [日記2025-09-03](../../raw/scrapbox_kozaneba/2025-09-03__日記2025-09-03.md)
- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md)
- [週記2025-09-14~2025-09-22](../../raw/scrapbox_kozaneba/2025-09-14__週記2025-09-14~2025-09-22.md)
- [日記2025-10-07](../../raw/scrapbox_kozaneba/2025-10-07__日記2025-10-07.md)
