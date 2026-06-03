---
title: Canvas 1 万件デモの拡張 — サイド実験プロトタイプを 2026-06 サーベイで再評価
type: theme
created: 2026-06-03
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md
  - raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md
  - wiki/themes/Canvas移行の検討.md
  - wiki/themes/畳むUIの再設計.md
  - wiki/concepts/なめらかな畳まれ.md
  - wiki/concepts/広聴AI.md
---

## このページの位置付け(重要)

このページが扱うのは **Kozaneba 本流(`work/kozaneba/`)の改修ではなく、独立したサイド実験プロトタイプ** の拡張可能性。具体的には 2025-08-26~27 に nishio + GPT-5/Claude が作った Canvas 版プロトタイプ(deploy: `https://canvas-kozaneba-prototype.vercel.app/`、[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md))。

このプロトタイプは:

- **Kozaneba 本流から独立した別実装**(本流のスキーマ・状態管理・Cypress テスト基盤には乗っていない)
- 動機は [広聴AI](../concepts/広聴AI.md) のリーフノード = 付箋化を試したいというサイド実験
- [3 Plan 議論](3plan議論.md) の **Plan A / Plan B のどちらにも属していない第三の試み**

したがって以下に書く「拡張」は、**(a) このプロトタイプ路線を続けるなら、または (b) プロトタイプの知見を本流側 / Plan A 側に持ち帰るなら**、という条件付きで読む。本流改修ロードマップではない。

## このテーマの問い

プロトタイプは「1 万枚付箋 / ズーム可 / 密度の付箋化」までは到達した。一方 2026-06-01 にまとめた [畳むUIの再設計](畳むUIの再設計.md) で外部文献を見ると、**semantic zoom / inverse-zoom title / AI 自動表札** など、当時のプロトタイプに **まだ載っていない** 仕掛けが揃っている。

このページは、2025-08 のプロトタイプを 2026-06 のサーベイで再評価し、「次に何を足すと一段上がるか」を整理する。

## 2025-08 プロトタイプの到達点(再掲)

[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md) と [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md):

- **データ**: [広聴AI](../concepts/広聴AI.md) 由来の意見テキストを embedding → UMAP で 2D 化
- **配置**: `NOTE_SIZE=120px` で割った整数格子へスナップ、同一グリッド衝突は **螺旋状に空きを探索**
- **可視化**: 「[密度の高さを大きさに変換](../concepts/密度の高さを大きさに変換して可視化.md)」 — 散布図では一点に潰れる塊が、付箋として面積を持つ
- **性能**: 1 万枚で **ズームに性能上の問題なし** を確認(Canvas 描画)
- **比較対象**: Kernel Density Estimation のヒートマップ — 同等のクラスタ発見能力はあるが、「**1 つ 1 つが意見**」を一般人に伝える力は付箋の方が強い、と nishio は判断([認知メタファのデザイン](認知メタファのデザイン.md))

ただし「**ズームしても個別付箋の中身は変わらない**(描画解像度が落ちるだけ)」状態に留まっている。これは [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) と同型で、**graphical zoom であって semantic zoom ではない**。

## サーベイから引き取れる 3 つの拡張(プロトタイプを続ける場合)

### 1. Heptabase 型 inverse-zoom title + 閾値で representation switch

[畳むUIの再設計](畳むUIの再設計.md) の中心提案。1 万件プロトタイプに当てはめると:

- 螺旋配置の「グリッドに集まった N 枚」を、ある閾値以下のズームでは **代表 1 枚だけ大きく描く**(残りの個別付箋は描画しない)
- 閾値を超えてズームインしたら、個別の付箋に展開
- これは Perlin & Fox の semantic zoom そのもの:**サイズの関数として表現が変わる**

現状の「密度 → 大きさ」と新規の「束 → 表札」が **同じ semantic zoom 軸で統一** され、ズームレベルが進むに連れて「**ぼんやり全体**」→「**塊の名前**」→「**個別の意見**」と情報粒度が連続変化する流れになる。

本流 Kozaneba 側で同じ semantic zoom を実装する判断は別物([畳むUIの再設計](畳むUIの再設計.md) を参照)。プロトタイプ側はゼロから書ける分だけ実装が軽い、というだけのこと。

### 2. AI 自動表札(Notion AI / Heptabase Tags 型)

代表 1 枚に何を書くか問題への直接解。

- 螺旋グリッドに溜まった N 枚を LLM で要約 → グリッド代表の表札にする
- 現状の「**密度の高い四角い塊**」が「**この塊は何の話か**」になる
- 広聴 AI 側の凝集クラスタリングと併用しても自然(**クラスタ ID = 表札の単位**)
- ユーザは override で書き換え可能、書き換えなければ AI が中身の変化に追随

[pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md) で nishio が指摘した「連結成分の矩形選択だとオーバーラップが発生」問題への別解にもなる — 矩形ではなく **「同じ表札に属する集合」を 1 単位** として扱えば、空間的なオーバーラップに依存しない。

[Clean Relation Elicitation](Clean_Relation_Elicitation.md) の Lift / Formulate と同方向: 表札 = グループの Aspect、AI による Formulate。

### 3. プロトタイプ側に first-class frame 抽象を入れる + Hierarchical Edge Bundling

[pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md) の重要な観察:

> やっぱ 10000 件以上のものをどうするかという路線と、1000 件未満の付箋を作りながら構造化していく Kozaneba は無闇に同一視しない方が良い気がするなぁ。

→ [人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) で展開。プロトタイプ側で「自動 frame + AI 表札」を入れるなら:

- 螺旋グリッドの集約単位を frame として一級化(プロトタイプ自身のデータモデルで)
- frame 間の線は [Hierarchical Edge Bundling (Holten 2006)](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf) で frame 階層に沿って束ねる

本流 Kozaneba 側でも独立に「group を first-class frame に格上げ」する案は [畳むUIの再設計](畳むUIの再設計.md) にある(小規模・人間駆動のため動機は別)。**両者を同じ frame 抽象に揃えるかは独立の判断** であり、自動的に揃えるべきものではない([人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) で隙間の意味が違うことを警告)。

## 流れの全体像(プロトタイプ側)

```
[広聴AI: 意見テキスト N=10000]
        ↓ embedding + UMAP
[2D 座標]
        ↓ NOTE_SIZE=120 グリッドスナップ
[格子点 + 重複カウント]
        ↓ 螺旋分散配置        ← 2025-08 のここまで
[付箋として描画(密度=面積)]
        ↓ LLM 要約             ← 拡張 2(AI 表札)
[各グリッドに代表表札]
        ↓ プロトタイプ側 frame 抽象   ← 拡張 3
[bounding_rect 付き frame]
        ↓ semantic zoom        ← 拡張 1(inverse-zoom title)
[ズーム閾値で 表札 ⇄ 個別 を切替]
```

## 本流側との接点 — 統合は別問題

プロトタイプ側の拡張は、本流 Kozaneba にどう跳ね返るか:

- **無関係に終わる場合**: プロトタイプは広聴 AI 用の見せ方ツールに進化、本流は本流で別の優先順位で進む([Plan B 試行 2026-06](Plan_B試行_2026-06.md)、本流の Plan B 系作業群)。これが現実的な default
- **知見だけ持ち帰る場合**: AI 表札 / inverse-zoom title / frame 抽象 は本流側でも価値があるかもしれないが、本流側で実装するかは本流側のロードマップ([3 Plan 議論](3plan議論.md) / [Plan B 試行 2026-06](Plan_B試行_2026-06.md))で別途決める
- **データモデルを統合する場合**: [3 Plan 議論](3plan議論.md) で立てた Plan A(新規サービス)の設計に取り込むのが自然。本流の `work/kozaneba/` を改造して大規模も飲み込ませる方向は、Plan B のスコープを大きく超えるので別判断が要る

[人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) が示す通り、本流(人間駆動の小規模)とプロトタイプ(アルゴ駆動の大規模)は **空間構造の生成者が違う** ので、frame 抽象を「同じ型」にすると意味が壊れる可能性がある。安易な統合は避ける。

## 残された問い

- プロトタイプは現状 deploy されているが、その後の積極的な開発は見えない。継続する動機は誰がどう持つか
- AI 表札を入れたとき、ユーザは「**AI が決めた表札**」をどこまで信頼して触らないか。Notion AI auto-labeling は手動 override 前提だが、広聴 AI の「**未だ意思決定されていない大量の声**」コンテキストで同じ前提が成り立つか未検証
- Kernel Density Estimation との比較で nishio は「**1 つ 1 つが意見** を伝えたい」を主理由に付箋を選んだ([認知メタファのデザイン](認知メタファのデザイン.md))。AI 表札を入れた瞬間に「**この塊は X の話**」の抽象度が上がり、KDE と同じ「データサイエンティスト向け」側に寄ってしまう可能性がある。これは表札の言葉遣い(具体性)で受けるべきか、representation switch の閾値で受けるべきか
- 「2 つの Kozaneba」を本流とプロトタイプの両方で別実装で進めると保守コストが二重化する。共通の **データ整形パイプライン(embedding / UMAP / クラスタリング)** だけは共有できるかもしれない

## 関連

- [Canvas移行の検討](Canvas移行の検討.md) — プロトタイプ本体と本流 Canvas 化議論
- [畳むUIの再設計](畳むUIの再設計.md) — semantic zoom / frame / AI 表札サーベイ(拡張案の出所)
- [なめらかな畳まれ](../concepts/なめらかな畳まれ.md) — graphical zoom に留まる現状認識
- [大きな付箋](../concepts/大きな付箋.md) — 人力 inverse-zoom title 相当
- [密度の高さを大きさに変換して可視化](../concepts/密度の高さを大きさに変換して可視化.md) — プロトタイプの可視化原則
- [認知メタファのデザイン](認知メタファのデザイン.md) — KDE ではなく付箋を選んだ根拠
- [人間が動かすから隙間ができる](人間が動かすから隙間ができる.md) — 本流とプロトタイプの本質的な違い
- [広聴AI](../concepts/広聴AI.md) — プロトタイプの動機
- [線UIサーベイ 2026](線UIサーベイ_2026.md) — Hierarchical Edge Bundling
- [3 Plan 議論](3plan議論.md) — プロトタイプはどの Plan にも属していない第三の試み

## Sources

- [pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)
- [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)
- [Hierarchical Edge Bundles — Holten 2006](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Holten06.pdf)
- [Pad: Perlin & Fox 1993](https://mrl.cs.nyu.edu/~perlin/pad-siggraph.pdf)
- [Heptabase Updates 2025-12-30: AI Action](https://wiki.heptabase.com/newsletters/2025-12-30)
- [Notion AI auto labeling](https://www.notion.com/help/guides/organize-your-inbox-with-notion-ai-auto-labeling)
