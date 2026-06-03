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

**広聴AI** は、[ブロードリスニング](ブロードリスニング.md) を AI で支援する具体的な実装/プロジェクト名。2025 年に nishio が手がけており、**大量の意見テキストを embedding して 2 次元に並べ、クラスタリングして可視化する** 仕組みを持つ。

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

2025-08-26~27 に nishio + GPT-5/Claude が、広聴AI のデータ(意見テキストを embedding + UMAP)を **Kozaneba 風付箋として 1 万件描く Canvas プロトタイプ** を作った([pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)、deploy: `canvas-kozaneba-prototype.vercel.app`)。

これは **Kozaneba 本流(`work/kozaneba/`)の改修ではなく独立した別実装** で、Plan A / Plan B のどちらにも属していない **第三のサイド実験**。広聴AI 側で「リーフノード = 単なる点 ではなく付箋」を試したいというニーズが動機。

このプロトタイプから抽出した一般概念:

- [密度の高さを大きさに変換して可視化](密度の高さを大きさに変換して可視化.md) — 中心可視化原則
- [認知メタファのデザイン](../themes/認知メタファのデザイン.md) — KDE ではなく付箋を選んだ根拠
- [人間が動かすから隙間ができる](../themes/人間が動かすから隙間ができる.md) — 本流(人間駆動)とプロトタイプ(アルゴ駆動)の本質的な違い
- [内部構造がわかりやすい](内部構造がわかりやすい.md) — 機能 vs 見栄えのトレードオフ

拡張案: [Canvas 1 万件デモの拡張](../themes/Canvas_1万件デモの拡張.md)。

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
