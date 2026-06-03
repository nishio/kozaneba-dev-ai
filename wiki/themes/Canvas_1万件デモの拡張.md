---
title: Canvas 1 万件プロトタイプ — 未解決の探索の途中で停止、サーベイ結論との関係
type: theme
created: 2026-06-03
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md
  - raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md
  - work/canvas_kozaneba_prototype/src/types.ts
  - work/canvas_kozaneba_prototype/src/StickyNotesClustersView.tsx
  - work/canvas_kozaneba_prototype/src/summarizer.ts
  - work/canvas_kozaneba_prototype/scripts/precompute_clusters.js
  - wiki/themes/Canvas移行の検討.md
  - wiki/themes/畳むUIの再設計.md
  - wiki/concepts/広聴AI.md
---

## このページの位置付け(重要)

このページが扱うのは **Kozaneba 本流(`work/kozaneba/`)の改修ではなく、独立したサイド実験プロトタイプ**(repo: `nishio/canvas_kozaneba_prototype`、deploy: `canvas-kozaneba-prototype.vercel.app`、local: `work/canvas_kozaneba_prototype/`)。

- **Kozaneba 本流から独立した別実装**(本流のスキーマ・状態管理・テスト基盤には乗っていない、Vite + TypeScript + Canvas API、約 2100 行)
- 動機は [広聴AI](../concepts/広聴AI.md) のリーフノード = 付箋化を試したいというサイド実験
- [3 Plan 議論](3plan議論.md) の **Plan A / Plan B のどちらにも属していない第三の試み**
- 2025-08-29 最終 commit、その後の更新なし

## このプロトタイプの実態(2026-06-03 コード + Scrapbox 再確認)

2026-06-03 にこのページを最初に書いたとき、私(LLM)はプロトタイプのコードを読まずに [畳むUIの再設計](畳むUIの再設計.md) のサーベイ結論を「拡張案」として提案していた。コードを読むと cluster sticky / AI 表札 / first-class frame の rendering ロジックはあったので「先取り」と書いたが、これも **二重に誇張** だった。

正確な実態:

1. **default UX には統合されていない**: deploy された `canvas-kozaneba-prototype.vercel.app/` を開くと出るのは `StickyNotesZoomDemo`(単に 1 万件付箋を LOD でズーム表示するだけ)。cluster sticky / AI 表札がある `StickyNotesClustersView` は **`#/clusters` という experimental サブルート**にしか居らず、ZoomDemo からの動線も無い
2. **クラスタ抽出方針自体が未解決で停止**: [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)(最終 commit 日のメモ)を再読すると、**連結成分 vs 10 マス割り** のどちらを採るかで悩んでいる途中で終わっている。「**たぶんこのあたりが連鎖的に繋がってしまうのだと思う**」が最後の一文
3. **路線統合判断も未解決**: 同日のメモに「**やっぱ 10000 件以上のものをどうするかという路線と、1000 件未満の付箋を作りながら構造化していく Kozaneba は無闇に同一視しない方が良い気がするなぁ**」とあり、そもそもこの方向を続けるべきかにも疑問が投げかけられている
4. その日が **最終 commit の日**(以降の更新なし)

つまりこのプロトタイプは **「設計判断の途中で止まった探索」** であって、「サーベイ結論を先取りした完成品」ではない。`StickyNotesClustersView.tsx` の cluster sticky rendering は **draft 段階の一バージョン**。

## 「先取り」の誇張を訂正する含意

- **アイデアレベルでは到達していた**(連結成分でクラスタ化、矩形を frame として扱う、AI で要約する、ズーム閾値で表示切替する)
- **コード上には rendering ロジックが存在する**(`StickyNotesClustersView.tsx`、`summarizer.ts`、`types.ts`)
- **しかし「完成」「移植可能な成功事例」ではない**:設計判断が未解決のまま停止、default UX に統合されていない
- [畳むUIの再設計](畳むUIの再設計.md) の本流改修案は「プロトタイプの draft を移植すれば済む」とは言えない。**プロトタイプが避けて通った設計判断(クラスタ抽出方針 / マージ閾値 / 路線統合判断)を本流側でも別途決める必要がある**

## コード上に存在する rendering ロジック(2025-08-29 時点、`#/clusters` 限定)

### A. データモデル: cluster as first-class frame

[`src/types.ts`](../../work/canvas_kozaneba_prototype/src/types.ts):

```ts
interface ClusterRect { x: number; y: number; w: number; h: number; }
interface ClusterSummary {
  id: string;
  rect: ClusterRect;          // ← first-class bounding rect
  noteIds: string[];
  texts: string[];
  summary?: string;           // ← AI 表札
}
```

[畳むUIの再設計](畳むUIの再設計.md) の「group を first-class frame に格上げ(`RTGroupItem` に `bounding_rect` 追加)」案がプロトタイプ側では **完成形** に近い。

### B. UMAP + 螺旋分散 + 「[密度の高さを大きさに変換して可視化](../concepts/密度の高さを大きさに変換して可視化.md)」

[`StickyNotesClustersView.tsx:25-83`](../../work/canvas_kozaneba_prototype/src/StickyNotesClustersView.tsx) と [`scripts/precompute_clusters.js:38-83`](../../work/canvas_kozaneba_prototype/scripts/precompute_clusters.js):

- `NOTE_SIZE = 120`, `POSITION_SCALE = 4000`
- 各意見の UMAP 座標を整数格子へスナップ
- 衝突は **半径 0..20 の同心リング** で空きを探索(`for (let radius = 0; radius < 20 && !found; radius++) ...`)
- 結果として密集が面積として可視化

Scrapbox に書かれていなかった具体値:`POSITION_SCALE=4000` は実装の default(Scrapbox では SCALE=600/2000/4000 の実験記録があったが、最終的に 4000 に固定された)。

### C. semantic zoom / inverse-zoom title — `screenNoteW >= 80` 閾値

[`StickyNotesClustersView.tsx:240, 304`](../../work/canvas_kozaneba_prototype/src/StickyNotesClustersView.tsx):

```ts
const NOTE_SIZE = 120
const screenNoteW = NOTE_SIZE * scale
const showText = screenNoteW >= 80
// ...
if (clusterRender === 'sticky') {
  if (screenNoteW >= 80) {
    // 通常付箋の文字が見えるズームでは、クラスタ大きな付箋は非表示
  } else {
    // クラスタを大きな付箋として描画(summary をタイトル表示)
  }
}
```

[畳むUIの再設計](畳むUIの再設計.md) の中心提案「**ある閾値で『子を描画しない、表札だけ』モードに遷移**」がそのまま実装されている。閾値は **画面上での個別付箋の見かけ幅 80px** で切替。

### D. AI 自動表札 — `summarizer.ts` + OpenRouter 経由 precompute

[`src/summarizer.ts`](../../work/canvas_kozaneba_prototype/src/summarizer.ts):

```ts
export async function summarizeTextsLLM(texts: string[]): Promise<string> {
  // 1) /api/summarize がフラグ立ってれば呼ぶ
  // 2) フォールバック: 上位 3 文 + トップ 8 キーワード抽出
}
```

[`scripts/precompute_clusters.js`](../../work/canvas_kozaneba_prototype/scripts/precompute_clusters.js):
- `--use-openrouter` フラグで OpenRouter の `openai/gpt-4o-mini`(`max-tokens=600`)で precompute
- 結果は `public/clusters_summary.json` に保存して static 配信
- ランタイムで非同期にロード、`ClusterSummary.summary` が表札タイトルとして描画される(`extractTitleFromSummary` で一文目を抽出)

「[畳むUIの再設計](畳むUIの再設計.md) > AI 自動表札」の Notion AI / Heptabase Tags 型の実装そのもの。ユーザ override の動線はないが、precompute → 静的 JSON → 必要なら再生成、というシンプルなパイプライン。

### E. 「重なっているもののマージと正方形化」 (commit `e542fc7`)

commit メッセージ:

> precompute: クラスタ矩形のオーバーラップ併合と正方形化を導入。併合→正方形化→再併合を収束まで反復し非重複化。最終クラスタ確定後に要約生成。

[pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md) の「重なっているもののマージと正方形化、いま 3 割以上重なっている場合のみマージ」の議論の収束版が実装に入っている。

## 「大きな付箋」用語の同名異物

プロトタイプの commit ログには「大きな付箋」が頻出するが、本流の [大きな付箋](../concepts/大きな付箋.md) とは **別物**:

| | 本流 | プロトタイプ |
|---|---|---|
| 単位 | 個別 こざね / group | cluster 全体 |
| 機構 | `item.scale` 増加([BigSmallMenuItem.tsx](../../work/kozaneba/src/Menu/BigSmallMenuItem.tsx) 操作) | `clusterRender = 'sticky'`(commit `d8f1053` で default 化) |
| 操作 | ユーザがメニューで big/twice/small/half | アルゴが自動生成、UI モード切替のみ |
| 思想 | ユーザが意思決定して上位構造を作る | アルゴが密集を検出して上位構造化 |

両者は **「ズームアウトでも読める単位を作る」思想で一致** しているが、実装と起点が違う。

## サーベイ結論との関係(誇張を訂正)

[畳むUIの再設計](畳むUIの再設計.md) は 2026-06-01 に外部文献(Pad++ / Bret Victor / Heptabase / tldraw frames / Notion AI / Magic Lens 等)から「**ある閾値で representation switch + frame as first-class + AI 自動表札**」という結論を導いた。

プロトタイプはこの結論に対応する rendering ロジックを **2025-08-29 時点でアイデアレベルでは draft として書いていた**。ただし上述の通り、これは「完成」でも「先取り」でもなく、**設計判断の途中で停止した探索コード** にすぎない。

含意としては:

1. **AI ペアプロは「コードを書くスピード」は速い**(英単語改行・サンプルデータ・絵文字トラップなど、過去 nishio が時間を投じた処理を一発で出す)
2. **しかし「何を作るべきか / どのアプローチが正しいか」の設計判断は AI が肩代わりしない**:nishio は連結成分 vs 10 マス割りの判断、マージ閾値の判断、10000 vs 1000 路線の分離判断、で止まっている
3. むしろ AI が実装を高速化すると、**設計判断のボトルネックが顕在化** する
4. **本流側に持ち帰るときも同じ設計判断が必要**:プロトタイプという「成功事例」を移植して済む話ではなく、本流が選んだ ユーザ駆動の文脈で同じ問題(クラスタをどう定義するか / どのレベルで束ねるか)を別途解く必要がある

これは [Plan B 試行 2026-06](Plan_B試行_2026-06.md) の「賢い AI Agent は過去実装を簡単に直す」仮説に **ニュアンスを加える**:速いのは事実だが、設計判断の悩みは残る。仮説検証はこの面の評価も含めてやる必要がある。

## 本当の残課題

プロトタイプが停止した時点で **未解決のまま残っている問い** と、サーベイから見えた **そもそもプロトタイプに無い軸** の両方:

### 0. nishio 自身が 2025-08-29 に投げた未解決の問い(と 2026-06-03 の着地状況)

- **クラスタ抽出方針**: 連結成分(オーバーラップが発生)vs 10 マスごとに割る(自然なクラスタが分割される)。どちらも欠点があり、結論なし
  - → **2026-06-03 着地方向**: 「Google Maps のメタファで開き直る、タイル方式でいい」([Google Maps メタファ](Google_Mapsメタファ.md))。分割を許容する代わりに、マージ閾値不要・予測可能・ユーザ認知コスト最小、を得る。「**自然なクラスタを正確に描く**」を諦める引き換えに **「タイルだから」というメタファで意味の欠如を正当化** する第三の道
- **マージ閾値**: 3 割 / 2 割 を実験、2 割だと連鎖マージが起きてしまう。最適値が見えていない
  - → タイル方式に切り替えれば **そもそも閾値が要らない**(連鎖マージが起こりえない)
- **路線統合判断**: 10000 件以上の路線と 1000 件未満の Kozaneba を「無闇に同一視しない方が良い」— つまりプロトタイプ路線を Kozaneba と統合すべきかにも疑問が投げかけられた
  - → 未解決のまま。Google Maps メタファは「本流の中心を変える」のではなく「**大規模データを本流に登場させるときの表現方法**」として活きる、と読むのが [Google Maps メタファ](Google_Mapsメタファ.md) の含意
- **default UX への統合**: cluster sticky を `#/clusters` に置いたまま、`/` の ZoomDemo からの動線を作っていない
  - → 未解決

これらは AI ペアプロでは肩代わりされない判断であり、プロトタイプ復活 or 本流移植のどちらでも先に解く必要がある。クラスタ抽出方針の着地で 1 つ前進したが、残りの 3 つは未解決。

### 以下、プロトタイプ単独でまだ未実装、または性質上不可能な軸:

### 1. 人間が動かす layer

[人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) で論じた根本的な未解決。プロトタイプは **アルゴ駆動の純粋形** で、人間がクラスタを「これは違う、こっちと一緒」と動かす UI は無い。広聴 AI の用途(まだ意思決定されていない大量の声を概観する)ではこれで足りる可能性もあるが、Kozaneba 系の「**人間が動かして意思決定する**」プロセスを追加するなら別設計。

### 2. Hierarchical Edge Bundling

クラスタ間の関係(線)はプロトタイプには無い。クラスタ間で多数の意見が relate するケース(同じ問題意識を別の角度から論じている等)を [Holten 2006](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf) で束ねる余地。

### 3. Magic Lens / Peek

ズーム閾値の切替だけでなく、**マウス位置の周辺だけクラスタを透過して中の付箋を覗く** UI(Bier et al. 1993)。「クラスタを開く/閉じる」より軽い覗き操作。プロトタイプにない。

### 4. AI 表札のユーザ override

現状の表札は precompute された読み取り専用。Notion AI auto-labeling のように **ユーザが書き換えると AI 提案を上書き、再生成時は手書きを優先** という動線は無い。これは Kozaneba 本流の「ユーザ意思決定の優先」設計思想([人間が動かすから隙間ができる](人間が動かすから隙間ができる.md))と合流させると価値がある。

### 5. 本流への持ち帰り

本流 [大きな付箋](../concepts/大きな付箋.md) は `item.scale` の人力操作で「ズームアウトでも読める単位」を作っている。プロトタイプの **(a) 閾値による representation switch、(b) AI 自動表札、(c) cluster as first-class frame** を本流側に移植するなら、[畳むUIの再設計](畳むUIの再設計.md) の改修案 1〜4 がそれに対応する。プロトタイプという「成功事例」が既に存在しているのは、本流改修の判断材料として強い。

## 開いた問い

- プロトタイプは 2025-08-29 以降更新がないが、広聴 AI 側で何が起こったか、Kozaneba 本流側がこの知見をどこまで取り込むかは [広聴AI](../concepts/広聴AI.md) の後続記述次第
- プロトタイプの `summarizer.ts` は OpenRouter `gpt-4o-mini` で precompute する設計。本流に持ち込むときは LLM コスト(クライアント API vs precompute vs WebLLM)を再評価する必要(cf. [畳むUIの再設計](畳むUIの再設計.md) の開いた問い)
- AI 表札を入れた瞬間に「**1 つ 1 つが意見**」の解像度が「**この塊は X**」の抽象度に上がる懸念([認知メタファのデザイン](認知メタファのデザイン.md))。プロトタイプ実装でこの問題が実際にどう出ているかは未確認(deploy を触ってみる必要)

## 関連

- [Canvas移行の検討](Canvas移行の検討.md) — プロトタイプ本体と本流 Canvas 化議論
- [畳むUIの再設計](畳むUIの再設計.md) — プロトタイプが先取りしていた結論の出所
- [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) — graphical zoom 止まりの本流現状
- [大きな付箋](../concepts/大きな付箋.md) — 本流とプロトタイプの同名異物
- [密度の高さを大きさに変換して可視化](../concepts/密度の高さを大きさに変換して可視化.md) — プロトタイプの可視化原則
- [認知メタファのデザイン](認知メタファのデザイン.md) — KDE ではなく付箋を選んだ根拠
- [人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) — プロトタイプに無い軸
- [広聴AI](../concepts/広聴AI.md) — プロトタイプの動機
- [線UIサーベイ 2026](線UIサーベイ_2026.md) — Hierarchical Edge Bundling
- [3 Plan 議論](3plan議論.md) — プロトタイプはどの Plan にも属していない第三の試み
- [Plan B 試行 2026-06](Plan_B試行_2026-06.md) — 「賢い AI Agent」仮説の傍証

## Sources

- [pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)
- [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)
- [work/canvas_kozaneba_prototype/src/types.ts](../../work/canvas_kozaneba_prototype/src/types.ts)
- [work/canvas_kozaneba_prototype/src/StickyNotesClustersView.tsx](../../work/canvas_kozaneba_prototype/src/StickyNotesClustersView.tsx)
- [work/canvas_kozaneba_prototype/src/summarizer.ts](../../work/canvas_kozaneba_prototype/src/summarizer.ts)
- [work/canvas_kozaneba_prototype/scripts/precompute_clusters.js](../../work/canvas_kozaneba_prototype/scripts/precompute_clusters.js)
- [Hierarchical Edge Bundles — Holten 2006](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf)
- [Toolglass and Magic Lenses — Bier et al. 1993](https://www.billbuxton.com/tgml93.html)
