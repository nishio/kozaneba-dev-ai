---
title: N項関係
type: concept
created: 2026-05-16
updated: 2026-06-02
sources:
  - raw/scrapbox_kozaneba/2021-08-10__pKozaneba.md
  - raw/関係UI議論_GPT.md
  - work/kozaneba/src/Global/TAnnotation.ts
---

## 定義

**N項関係**(n-ary relation)は、2つの項のあいだの関係(二項関係)を一般化し、任意個の項を含む関係。グラフ理論ではハイパーエッジに対応。

[Kozaneba](../entities/Kozaneba.md) の [線を引く機能](線を引く機能.md) は内部実装としては「N 個の要素の関係」を扱える設計になっている([pKozaneba](../../raw/scrapbox_kozaneba/2021-08-10__pKozaneba.md))。

## 実例

二項では表せない関係は多い:

- 「A は B の原因である」+「ただし条件 C のもとで」+「根拠は実験 D」+「ただし E に反証された」
- 「Keichobot の質問キーワード X と回答キーワード Y のペアが、文脈 Z で出現した」
- 「販売者 P が商品 G を買い手 B に価格 V で時刻 T に売った」(W3C N-ary Relations 例)
- 「(始点, 関係種別, 終点)」の三項関係 = [辺ラベル](辺ラベル.md) 付きの線

## コード上の現状(2026-05)

[work/kozaneba コード構造調査](../sources/kozaneba-code-architecture.md) で `RTLineAnnot` を確認した結果、Kozaneba の line annotation スキーマは:

```ts
items: Array(RTItemId)            // 長さ制限なし
heads: Array(RTArrowHead)          // items と同じ長さで並列、各端点の矢印頭
is_doubled: Boolean
label: String.optional()
```

つまり **データレイヤでは N 個の項を結ぶ関係が表現可能**(`items.length` に制限なし、各端点に矢印頭/無しを指定可能)。実用上「N項関係 UI」が無いのではなく、**「`items[]` を 3 以上にする UI 動線」が無い**だけ、と書き換えた方が正確。データモデル拡張を待たずに UI 実験ができる土台はある。

ただし [関係場](関係場.md) で論じた「項が状況からの分節として出てくる」を扱うには、依然として `items: Array(RTItemId)` 形式そのものが「項が先にある」前提を引きずっており、形式主義から逃げきれない。スキーマがリッチでも、本質的な批判は残る。

## Kozaneba での実装と「先回りした一般化」

[線を引く機能](線を引く機能.md) は当初「1:1 に限らない関係も表現できるべき」という意図で N項関係として実装された。しかし [活用されなかった機能](../themes/活用されなかった機能.md) で論じたように、実用上は別の特殊化された使われ方に流れた:

- 「1:N で同じ性質の矢印が発生する」(目次の章と節など)
- もっと単純な 1:1 関係
- 一般化しすぎた N項関係の UI は使いにくかった

## 「N項関係でもまだ足りない」(2026-05)

[raw/関係UI議論_GPT.md](../../raw/関係UI議論_GPT.md) の Round 3 で、[Gendlin](../entities/Gendlin.md) の関係論と照合した結果、N項関係は二項関係よりリッチだが **まだ「項が先にあって関係がそれらを結ぶ」という形式主義を残している** と指摘された:

> R(A, B, C, D) という形式は、A, B, C, D が先にあり、それらの間に R がある、という見方を残している。

ジェンドリン的には:

```
未分節な状況 S → 側面 α の持ち上げ → 項 A, B, C, D が現れる → 関係 R がまとまる
```

つまり項は状況からの分節として後から取り出される。これを保持するには [関係場](関係場.md) という上位概念が必要。

## 既存系譜での扱い

- **W3C N-ary Relations**: 通常の RDF は二項関係なので、追加情報や複数参加者が必要な関係は関係インスタンスを作る方法を推奨
- **Wikidata**: statement に qualifier、reference、rank を付けて文脈化
- **RDF 1.2**: triple term で「文について文を述べる」を標準化
- **ハイパーグラフ研究**: 二項分解では高次の依存関係を保てないと論じられる

## 2026-06: ジャンクションノード描画への切替案

[線UIサーベイ 2026](../themes/線UIサーベイ_2026.md) で hypergraph 可視化の研究系を見たところ、mainstream の whiteboard ツールは hypergraph をネイティブに扱わず、研究系は二つの表現を使う:

1. **Polygon / convex-hull 描画**: N項関係 = 多角形、各頂点がメンバー
2. **Bipartite / 中間ノード追加**: 「関係ノード」を 1 つ立て、各メンバーと二項線で結ぶ(de facto)

Kozaneba の `items: TItemId[]` モデルは **bipartite 表現と直接対応**。データは現状のまま、レンダリングを **中央ジャンクション + 放射状の線** に変えるだけで、N項関係が「中央の小ノード = 関係本体 / 周囲の線 = メンバー」として視覚的に成立する。これは [関係場](関係場.md) を一級可視化する道筋にもなる。

## 関連

- [線を引く機能](線を引く機能.md) — Kozaneba での N項関係実装
- [辺ラベル](辺ラベル.md) — N項関係の一形態(三項)
- [関係場](関係場.md) — N項関係の限界を超える概念
- [活用されなかった機能](../themes/活用されなかった機能.md) — N項関係 UI が実用化されなかった話
- [関係を第一級にする](../themes/関係を第一級にする.md)
- [線UIサーベイ 2026](../themes/線UIサーベイ_2026.md) — ジャンクションノード描画案と hypergraph 文献

## Sources

- [pKozaneba](../../raw/scrapbox_kozaneba/2021-08-10__pKozaneba.md) — `[辺ラベルは三項関係]` の言及
- [raw/関係UI議論_GPT.md](../../raw/関係UI議論_GPT.md) — N項関係 vs 関係場の議論
