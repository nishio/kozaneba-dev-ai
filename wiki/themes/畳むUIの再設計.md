---
title: 畳む UI の再設計 — Semantic Zoom / Frame / Magic Lens / AI 表札
type: theme
created: 2026-06-01
updated: 2026-06-03
sources:
  - 2026-06-01 のセッション(nishio による問題提起)
  - work/kozaneba/src/Global/TItem.ts (group)
  - 外部文献(Pad++ / Bret Victor / tldraw frames / Figma / FigJam / Miro / Heptabase / Muse / Kosmik / Obsidian Canvas / Apple Freeform / Workflowy / Tana / Magic Lens / Hierarchical Edge Bundling / Notion AI)
---

## このテーマの問い

[Kozaneba](../entities/Kozaneba.md) の「囲んで畳む UI」は紙の [KJ法](../concepts/KJ法.md) / [表札をつけて束ねる](../concepts/表札をつけて束ねる.md) を素直にデジタル化したものだが、実際にはほとんど使われなくなった([活用されなかった機能](活用されなかった機能.md))。

nishio 自身の振り返り(2026-06-01):

> 紙の KJ法 をエミュレートする上ではこれが重要だと思ってつけたが、使い勝手が悪かったのであまり使わなくなった。必要がないのかと言えば、そうではないはず。拡大した付箋とズームとで擬似的に解決されているが「畳むことによって情報量が減る」という効果を活用できていない。

二つの問題が独立にある:

- **(a) 操作上の問題**: 畳むと隙間が空き、開くと隣と被る
- **(b) 概念上の問題**: 「ズームアウトで読めなくする」[なめらかな畳まれ](../concepts/なめらかな畳まれ.md) は、見た目だけの圧縮であって **情報量は減っていない**(描画解像度が落ちるだけ)

この 2 つを外側の文献(ZUI 研究、modern thought canvas、AI native ツール)はどう解いているかを見る。

## 概念的な分類

「畳む」を解決する系譜は 4 つに分かれる。

### A. Semantic Zoom(Pad++ から Bret Victor まで)

**[Perlin & Fox "Pad: an alternative approach to the computer interface"](https://mrl.cs.nyu.edu/~perlin/pad-siggraph.pdf)**(SIGGRAPH '93)が起源。David Fox 由来の決定的アイデアは:**オブジェクトの表現は画面上のサイズの関数である** — 「成長する点は、まずシンプルなボックスに、次に 1 単語のラベル付きボックスに、次に長いラベルに、次にテキストと画像で満たされた矩形になる」。

**[Pad++ (Bederson & Hollan 1994)](https://en.wikipedia.org/wiki/Zooming_user_interface)** はこれを一般化、Magic Lens と組合せて複数表現の共存を可能にした。重要な性質:semantic zoom は「**グラフィカル表現のパラメータだけでなく、表示するデータの選択と構造そのものを変える**」。Kozaneba の [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) は **graphical zoom** に留まっており、semantic zoom に到達していない。

**[Bret Victor "Magic Ink"](https://worrydream.com/MagicInk/)**(2006)はこれを思想にまで上げた:大半のソフトウェアは情報ソフトウェアであり、デザイナの仕事は **「ユーザの今の context にふさわしい view を浮上させること」**。インタラクションは last resort、context(空間的・時間的)が優先。**畳む = ユーザコマンド** ではなく **畳まれた状態 = context から推論された view** という枠組みになる。

### B. Frame as first-class container

**[tldraw frames](https://tldraw.dev/sdk-features/shapes)**(2024)、**[Figma frames](https://help.figma.com/hc/en-us/articles/360041539473)**、**[FigJam sections](https://help.figma.com/hc/en-us/articles/4939765379351)**、**[Miro frames](https://help.miro.com/hc/en-us/articles/360018261813)**。グループが「独自の bounding rect / 座標系 / clip 領域 を持つ first-class object」になる。

Kozaneba の現状の `group` Item は `items: TItemId[]` を持つだけで、**畳まれた状態でも展開状態でも自前の bounding rect を持たない**。畳むと「中身の付箋がいた領域」が空白として残り、これが「隙間が空く」の正体。

**Figma の auto-layout**([Guide to auto layout](https://help.figma.com/hc/en-us/articles/360040451373))は frame が auto-layout モードのとき、子の追加/削除/サイズ変更で **frame 自身が自動で縮む/伸びる**。これが「畳んだら frame が縮む / 開いたら周囲を押しのける」の自然な実装。

**[Kosmik frames](https://www.kosmik.app/blog/kosmik-frames)**(2024)は「frame タイトルが全ズームレベルで visible」を売りにしている。**畳んだとき = タイトルだけ大きく見える / 開いたとき = タイトル + 中身** という二状態を frame の自然な性質に込めている。

### C. Recursive / nested canvas

**[Heptabase](https://wiki.heptabase.com/fundamental-elements)** が代表例。**Section**(whiteboard 内のグループ)と **Nested Whiteboards**(card 自身が別の whiteboard を持つ)を併用。公式の説明:

> When the whiteboard is zoomed out, you only see the Section group names, and all other data becomes tiny.

つまり **section の名前は inverse zoom で大きくなる** — これは Kozaneba の「大きな付箋」+ なめらかな畳まれ を **section title として一級化** したパターン。section の bounding rect が保持されるので隙間問題は発生しない。

**[Muse](https://museapp.com/memos/2020-12-infinite-canvas/)** は「**infinite nested boards**」を中心に置く。card を開く = 新しい sub-canvas に降りる、という再帰的設計。「畳んだ親と展開した子の同時操作」を避け、**視点切替** で扱う。

**[Obsidian Canvas](https://forum.obsidian.md/t/canvas-collapsable-groups/49542)** は native の collapse なし。Advanced Canvas / Collapse Node プラグインが補う。**Apple Freeform** は group が move-only(畳みなし)。

### D. Magic Lens / Portal(覗き窓)

**[Bier, Stone et al. "Toolglass and Magic Lenses"](https://www.billbuxton.com/tgml93.html)**(SIGGRAPH '93)。**透過する領域がその下の表現を変える** UI。畳まれたグループを **物理的に開かずに**「レンズを当てて中を覗く」操作で部分露出。

これは Kozaneba の「**開くと隣と被る**」問題への直接的回答:**開かなければ被らない**。閲覧目的なら開く必要すらなく、レンズで足りる。再配置目的のときだけ開く、と分離できる。

## アウトライナの「畳む」がなぜ隙間を作らないか

**[Workflowy](https://workflowy.zendesk.com/hc/en-us/articles/4410227171860) / Roam / Logseq / Tana** は 1D 垂直 flow で畳む。畳まれた節の下にあった全ブロックが上に詰まる。**第二の自由軸が無い** ので隙間ができない。

→ 2D canvas への素朴な移植は **空間情報を破壊する**(「このグループは左にある、なぜなら緊張関係にあるから」のような意味を失う)。outliner の解決法は **そのままは使えない**。ただし「グループ内部だけ auto-layout」で **局所的に同じ性質** を得られる(Figma frame の中だけ reflow が起きるイメージ)。

## AI 自動表札 — 2024-26 のフロンティア

「畳むことによって情報量が減る」という効果を **AI に表札を作らせる** ことで活かす道。

- **[Heptabase の AI Action / AI Insight](https://wiki.heptabase.com/newsletters/2025-12-30)**(2025-12 更新): card content から **自動タグ生成 / 要約生成 / 長文を ~300 文字単位に分割して個別要約**。要約は **派生 view** であって元データの一部ではない、再生成可能。
- **[Notion AI auto-labeling](https://www.notion.com/help/guides/organize-your-inbox-with-notion-ai-auto-labeling)**: database property(Summary / Key info / Status)を自動生成。「**summary as property**」という pattern は、Kozaneba の表札にそのまま使える: 表札 = フォールド時に AI が生成する **派生プロパティ**。
- **[Miro AI Mind Map](https://miro.com/ai/mind-map-ai/)**: AI 生成の collapsible 枝。collapse で AI 生成詳細を隠す、expand で復元。

Kozaneba 文脈での意義は二重:

1. **手で表札を書く負担が消える** → 紙の KJ法 で重かった「表札を書く」工程が UX 上ほぼ消える
2. **表札が動的に再生成される** → 中のこざねが変わると表札も自動で追随、紙では不可能な性質

## 比較表

| ツール / 概念 | グループ畳む? | 畳むと reflow? | Semantic zoom? | AI 表札? | 再帰 canvas? |
|---|---|---|---|---|---|
| Pad++ / Piccolo | n/a | n/a | **canonical** | × | yes |
| tldraw frames | clip + move | × | partial | × | flat |
| Figma frames | hug-content | **yes (auto-layout)** | × | × | nest |
| FigJam sections | hide/show | × | × | × | flat |
| Miro frames | native fold なし | × | feature request 段階 | × | × |
| **Heptabase** | section + nested wb | × (section title が大きくなる) | **yes** | **yes (Tags / AI Action)** | **yes** |
| Muse | nested board | n/a(sub-board へ降りる) | yes | partial | yes |
| Kosmik | frame(nest はまだ) | × | title 常時可視 | yes(moodboard) | partial |
| Obsidian Canvas | プラグインのみ | プラグインのみ | × | × | flat |
| Apple Freeform | move-only group | × | × | × | flat |
| Outliner(Workflowy / Roam / Tana) | yes | **yes (1D flow)** | × | partial | tree |

## Kozaneba への含意

優先度順:

### 1. group を「自前 bounding rect を持つ frame」に格上げ

これが **隙間問題の根本原因への直接対応**。`RTGroupItem` に `bounding_rect: { width, height }` を追加し、畳まれた状態でもこの矩形が空間に占有する。子要素は **rect 内部の座標系** で位置を持つ(現状は同じグローバル座標系で持っているはず — 要確認)。tldraw frames / Figma frame と同型。

これだけで「畳んだら隙間 / 開いたら被る」の両方が解ける(畳んでも frame は同じサイズ、開くと frame は中身に合わせて成長、周囲は局所 auto-layout で押される)。

### 2. Heptabase 型の「inverse zoom title」を実装

`group.text`(表札テキスト)を **ズームレベルの関数として描画**:ズームアウトすると表札フォントが相対的に大きくなり、子のこざねが小さくなる。ある閾値で **「子を描画しない、表札だけ」モードに遷移**。これは [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) の改良版で、**情報量がはっきり減る**(描画解像度の問題ではなく、明示的な representation switch)。

これは Perlin & Fox の semantic zoom そのもの: **サイズの関数として表現が変わる**。

### 3. AI 自動表札を入れる

`group.text` をユーザが書く代わりに、**LLM が中のこざねから自動生成**(Notion AI summary パターン)。ユーザは override で書き換え可能、書き換えなければ AI が動的に追随。これで「**表札を書くのが面倒** + **書いたあと中身が変わると表札が古くなる**」の二重問題が解ける。

[Clean Relation Elicitation](Clean_Relation_Elicitation.md) の Lift / Formulate 操作と並走できる: 表札 = group の Aspect、AI による Formulate。

### 4. Magic Lens で「開かずに覗く」

frame に対する transient lens を実装(右クリック → Peek、または特定キー押下中の hover で発動)。lens の下だけ frame が透過して中身が見える。**開かなくて済むケースがほとんど**(閲覧目的の確認)を吸収して、開閉操作の頻度自体を下げる。

これは Bier et al. の Magic Lens の素朴な移植。実装コストは中程度(SVG / Canvas のクリップ + 限定描画)。

### 5. 折りたたまれたグループ間のエッジは bundle

**[Hierarchical Edge Bundling (Holten 2006)](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf)**: 折りたたまれた階層上で、子同士のエッジを **親の階層にそって束ねて描く** 可視化技法。Kozaneba で「畳まれた group A と B の間に多数の線がある」状態を、**「A↔B 間の太い 1 本の線」+ 開いたときに展開** に縮約できる。線が情報量として残る一方で見やすくなる、relational fold への自然な拡張。

### 6. recursive canvas は「次フェーズ」として保留

Heptabase / Muse 型の **nested whiteboard**(group がそのまま sub-canvas になる)は強力だが、Kozaneba の「2D で発想を整理する」中心軸を変える大改修。**まず 1-4 をやって**、それでも階層が深くなりすぎたら検討。

## Bret Victor 流に問い直す

Magic Ink の枠組みで本テーマを言い直すと:

- 現状の「囲んで畳む」は **interaction** (ユーザがコマンドを発行する)
- 望ましいのは **context-driven view** (システムが context から view を推論)
- Context として使えるもの: **zoom level**(空間)、**focus / 最近編集**(時間)、**意味的近接**(LLM 計算)

「**畳む / 開く** を明示的 command として残しつつ、context によって **自動的に何が見えるかが変わる**」というハイブリッド設計が現実解。完全自動は [物理演算](../concepts/物理演算.md) と同じく「機械が動かしてはいけない」の禁忌に触れる可能性が高い。

## 広聴 AI 1 万件プロトタイプの先取り事例(本流ではなくサイド実験側)

[Canvas移行の検討](Canvas移行の検討.md) で扱った 2025-08 のプロトタイプ([pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)、repo: `nishio/canvas_kozaneba_prototype`)は **本流 Kozaneba ではなく [広聴AI](../concepts/広聴AI.md) 由来の独立サイド実験**。

本ページが 2026-06-01 に外部文献から導いた中心提案(inverse-zoom title / AI 自動表札 / first-class frame)に対応する rendering ロジックが、2026-06-03 のコード確認でプロトタイプ内に見つかった:

- **inverse-zoom title**: `screenNoteW >= 80` 閾値で「個別付箋表示」と「クラスタ大きな付箋表示」を切替(`StickyNotesClustersView.tsx:304`)
- **AI 自動表札**: `summarizer.ts` の `/api/summarize` 呼び出し + `precompute_clusters.js` の OpenRouter `gpt-4o-mini`(`max-tokens=600`)で precompute、`ClusterSummary.summary` として描画
- **first-class frame**: `types.ts:32` `ClusterSummary { id, rect: ClusterRect, noteIds, texts, summary? }`

ただし「先取り完成品」と判断するのは誇張で、**正確には**:

- **default UX には未統合**:`canvas-kozaneba-prototype.vercel.app/` のデフォルトは `StickyNotesZoomDemo`(LOD だけ)で、上記は `#/clusters` という experimental サブルートにしかない
- **設計判断が未解決のまま停止**:[pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md) で nishio はクラスタ抽出方針(連結成分 vs 10 マス割り)、マージ閾値、路線統合可否で悩んでいる途中で開発を止めた
- つまり cluster sticky rendering は **draft 段階の一バージョン**

含意としては、本ページの本流改修案 1〜3 は「プロトタイプの draft を移植する」だけでは済まず、**プロトタイプが避けて通った設計判断(どう束ねるか / 閾値 / 統合可否)を本流側でも別途解く必要がある**。

しかも [人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) で示すとおり、**アルゴ生成 frame(プロトタイプ)と人間配置 frame(本流)では「隙間の意味」が違う**(前者は副産物、後者は意思決定の投影)ため、frame 抽象を機械的に揃えると意味が壊れる可能性がある。nishio 本人が「**10000 件路線と 1000 件路線を無闇に同一視しない方が良い**」と最終 commit 日に言って止まったのは、この差を当事者として体感した結果と読める。

詳細は [Canvas 1 万件デモの拡張](Canvas_1万件デモの拡張.md)。

## 開いた問い

- 現状 `RTGroupItem` の子は **グローバル座標** か **group 相対座標** か(コード未確認、Plan B 改修の起点として要確認)
- AI 自動表札は LLM 呼び出しコストが場あたり累積する。クライアント側 LLM(WebLLM 等)で十分か、API 必須か
- Magic Lens は touch UI(iPad など)で操作がぎこちなくなる。代替ジェスチャ(long-press + drag、二本指)を考える必要がある
- [物理演算](../concepts/物理演算.md) の「機械が動かす」禁忌と、auto-layout reflow / AI 自動表札 はどこで線引きするか
- Kozaneba の「ねりねり」と相性が悪い操作はないか:nested canvas に降りたら「ねりねり」の感覚が切れる、frame で固定すると流動性が落ちる、等の副作用

## 関連

- [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) — 本ページの semantic zoom 提案は、これを明示化・強化したもの
- [表札をつけて束ねる](../concepts/表札をつけて束ねる.md) — KJ法 由来の操作、AI 自動表札はその省力化
- [活用されなかった機能](活用されなかった機能.md) — 「囲んで畳む」失敗の経緯
- [物理演算](../concepts/物理演算.md) — auto-layout reflow との緊張関係
- [Clean Relation Elicitation](Clean_Relation_Elicitation.md) — AI による Formulate / 表札生成
- [Canvas移行の検討](Canvas移行の検討.md) — レンダリング層改修との並行作業
- [Canvas 1 万件デモの拡張](Canvas_1万件デモの拡張.md) — 本ページの提案を広聴 AI 由来の 1 万件プロトタイプに適用
- [3 Plan 議論](3plan議論.md) — Plan B 改造の優先順位
- [データモデル刷新の選択肢](データモデル刷新の選択肢.md) — 同時期のデータ層サーベイ
- [線UIサーベイ 2026](線UIサーベイ_2026.md) — 同時期の UI サーベイ
- [kozaneba-code-architecture](../sources/kozaneba-code-architecture.md) — 現状 group 実装

## Sources

- [Pad: Perlin & Fox 1993](https://mrl.cs.nyu.edu/~perlin/pad-siggraph.pdf)
- [Zooming user interface (Wikipedia)](https://en.wikipedia.org/wiki/Zooming_user_interface)
- [Semantic Zoom (InfoVis Wiki)](https://infovis-wiki.net/wiki/Semantic_Zoom)
- [Zoom In, Zoom Out — Bederson, CACM 2012](https://cacm.acm.org/magazines/2012/12/157882-zoom-in-zoom-out/fulltext)
- [Bret Victor "Magic Ink"](https://worrydream.com/MagicInk/)
- [Toolglass and Magic Lenses — Bier et al. 1993](https://www.billbuxton.com/tgml93.html)
- [Hierarchical Edge Bundles — Holten 2006](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf)
- [Semantic Zoom for Software Cities — arXiv 2510.00003](https://arxiv.org/pdf/2510.00003)
- [tldraw Shapes docs](https://tldraw.dev/sdk-features/shapes)
- [Figma Guide to auto layout](https://help.figma.com/hc/en-us/articles/360040451373)
- [Figma Frames in Figma Design](https://help.figma.com/hc/en-us/articles/360041539473)
- [FigJam sections](https://help.figma.com/hc/en-us/articles/4939765379351)
- [Miro Frames](https://help.miro.com/hc/en-us/articles/360018261813)
- [Heptabase Fundamental Elements](https://wiki.heptabase.com/fundamental-elements)
- [Heptabase UI Logic](https://wiki.heptabase.com/user-interface-logic)
- [Heptabase Updates 2025-12-30: AI Action](https://wiki.heptabase.com/newsletters/2025-12-30)
- [Muse Infinite Canvas](https://museapp.com/memos/2020-12-infinite-canvas/)
- [Kosmik Frames](https://www.kosmik.app/blog/kosmik-frames)
- [Obsidian Canvas collapsible groups](https://forum.obsidian.md/t/canvas-collapsable-groups/49542)
- [Workflowy Expand & collapse all](https://workflowy.zendesk.com/hc/en-us/articles/4410227171860)
- [Notion AI auto labeling](https://www.notion.com/help/guides/organize-your-inbox-with-notion-ai-auto-labeling)
- [Miro AI Mind Map Generator](https://miro.com/ai/mind-map-ai/)
- [kozaneba-code-architecture](../sources/kozaneba-code-architecture.md)
