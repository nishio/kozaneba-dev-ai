---
title: llm-wiki クロスリファレンス
type: meta
created: 2026-05-16
updated: 2026-05-16
sources:
  - /Users/nishio/llm-wiki/wiki/
---

# llm-wiki クロスリファレンス

## このページは何か

nishio が並行して育てている [llm-wiki](../../../llm-wiki/wiki/index.md)(知的生産・LLM Wikiパターンに関する知識ベース)の中から、Kozaneba プロジェクトと接続する論点を抽出した索引。

llm-wiki 側はすでに Kozaneba を比較対象として継続参照しており、Kozaneba を観察した結果として一般化された概念ページ群が育っている。本ページは**「どこを参照すべきか」「どこから取り込むべきか」の地図**として機能する。

実際の取り込み(ingest)は別途、対応する Kozaneba 側ページに反映する形で行う。本ページはそのための作業台。

## llm-wiki 側の Kozaneba 直接言及ページ

### Kozaneba 本体・関連システム

| llm-wiki 側ページ | 内容 |
|---|---|
| [entities/kozaneba](../../../llm-wiki/wiki/entities/kozaneba.md) | Kozaneba を MindTrellis / ConnectingDots と並べて比較。AI を入れる場合の設計案あり |
| [entities/connecting-dots](../../../llm-wiki/wiki/entities/connecting-dots.md) | nishio 設計の Dots/Relations/Stories/Views 4 層モデル。Kozaneba を「Story 編集の View の一種」として位置づけ |
| [entities/mindtrellis](../../../llm-wiki/wiki/entities/mindtrellis.md) | 共同編集可能な知識グラフ研究システム(arXiv 2604.23129)。3 エージェント構成(Oracle / Adaptive Retriever / Map Manager) |
| [sources/gpt-mindtrellis-connectingdots-20260503](../../../llm-wiki/wiki/sources/gpt-mindtrellis-connectingdots-20260503.md) | 上記比較の元になった ChatGPT 5 ターン会話。Kozaneba+AI の 4 週間アクションプラン含む |

### Kozaneba 観察から抽出された概念

| llm-wiki 側ページ | 抽出された主張 |
|---|---|
| [concepts/pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md) | 「Kozaneba は前言語的な構造化に強い」を 3 Wiki が独立に再発見した結論。「まだ言語化できない近さ」を扱える設計 |
| [concepts/kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md) | 川喜田二郎の後期手法「考える花火」と LLM Wiki の同型分析。Kozaneba を「考える花火型デジタルツール」として明示参照 |
| [concepts/structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md) | AI が出した構造を仮説扱いし人間が違和感の差分を編集。「Kozaneba に足すべき AI はこざね・関係・反対例を差し出す AI」 |

## Kozaneba 側ページ → llm-wiki 側で効きそうな参照

下表は **Kozaneba 側の既存ページに対して、llm-wiki 側のどのページが理論的補強・対比材料になるか**の対応表。

### 関係性まわり(現在進行中の中心論点)

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [themes/関係を第一級にする](../themes/関係を第一級にする.md) | [relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md)(Gendlin / KJ 法的「あてはめ民主主義」批判の現代版定式化、TransE/RotatE 等の埋め込み議論) / [concept-as-region](../../../llm-wiki/wiki/concepts/concept-as-region.md)(Gärdenfors 概念空間で「点ではなく領域+演算」の対案) |
| [concepts/辺ラベル](../concepts/辺ラベル.md) | [relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md) の「ラベルは取っ手であって答えではない」議論、`typed-links` の 8 動詞最小語彙 |
| [concepts/N項関係](../concepts/N項関係.md) | [concept-as-region](../../../llm-wiki/wiki/concepts/concept-as-region.md) の「目的のために一時的に切り出す」概念観 |
| [concepts/関係場](../concepts/関係場.md) | [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md)(Situation 層は前言語的レイヤーに対応) / [relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md) |
| [concepts/毛玉問題](../concepts/毛玉問題.md) | [relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md)(分節過程の消去 = 平坦化の症状) / [bridge-vs-hybrid](../../../llm-wiki/wiki/concepts/bridge-vs-hybrid.md)(中心性指標では区別できない) |
| [themes/Clean Relation Elicitation](../themes/Clean_Relation_Elicitation.md) | [structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md)(候補は AI、確定は人間)/ ConnectingDots の Inbox 化パイプライン |

### 読書・思考整理の使い方

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [concepts/Kozaneba読書](../concepts/Kozaneba読書.md) | [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md) / [kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md)(8 段階手順は川喜田の手順と同型) |
| [concepts/ねりねり](../concepts/ねりねり.md) / [概念/既存の構造の破壊](../concepts/既存の構造の破壊.md) | [structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md)(編集行為が理解を生む) |
| [themes/哲学書読解の実験](../themes/哲学書読解の実験.md) | [kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md) / [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md) |
| [concepts/フェルトセンス](../concepts/フェルトセンス.md) / [concepts/dwell-think](../concepts/dwell-think.md) | [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md)(Gendlin 由来の前言語層の保護) |

### KJ法系列

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [concepts/KJ法](../concepts/KJ法.md) / [concepts/累積KJ法](../concepts/累積KJ法.md) / [概念/繰り返しKJ法](../concepts/繰り返しKJ法.md) | [kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md)(KJ 法 → 探検ネット → 考える花火 → Keichobot の系譜整理) |
| [concepts/渾沌をして語らしめる](../concepts/渾沌をして語らしめる.md) | [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md) / `fact-wiki-separation`(データに従属させる原理) |
| [concepts/表札をつけて束ねる](../concepts/表札をつけて束ねる.md) | [kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md) の「表札づくり」フェーズ |
| [entities/川喜田二郎](../entities/川喜田二郎.md) | [sources/nishio-kangaeru-hanabi](../../../llm-wiki/wiki/sources/nishio-kangaeru-hanabi.md)(後期手法と Keichobot/LLM Wiki への接続) |

### データモデル・「源の長文」周辺

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [concepts/源の長文](../concepts/源の長文.md) | [connecting-dots](../../../llm-wiki/wiki/entities/connecting-dots.md) の Dots(検証可能な事実)層との対応。Kozaneba こざね ≒ Aspect、源の長文 ≒ Situation/Dots 由来側 |
| [concepts/こざね](../concepts/こざね.md) / [concepts/Scrapboxこざね](../concepts/Scrapboxこざね.md) | [identity-without-name](../../../llm-wiki/wiki/concepts/identity-without-name.md)(デライト知番のように名前と対象の同一性を分離。こざねの ID 設計に転用可能) |
| [themes/活用されなかった機能](../themes/活用されなかった機能.md) | `narrative-value` / `narrative-loss`(wiki 化で抜けるナラティブ 6 次元。設計判断の文脈消失への注意) |

### Kozaneba の代替・隣接ツール

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [themes/Canvas移行の検討](../themes/Canvas移行の検討.md) | [structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md) / [connecting-dots](../../../llm-wiki/wiki/entities/connecting-dots.md)(AI を入れる時の「候補/確定」分離) |
| [entities/Miro](../entities/Miro.md) | [entities/mindtrellis](../../../llm-wiki/wiki/entities/mindtrellis.md)(空間配置 + AI 編集の最近の研究系統) |
| [entities/Scrapbox](../entities/Scrapbox.md) | [sources/gpt-delite-cosense-llmwiki-20260514](../../../llm-wiki/wiki/sources/gpt-delite-cosense-llmwiki-20260514.md)(Cosense・デライト・Karpathy LLM Wiki の三者比較) |

### Plurality / ブロードリスニング系

| Kozaneba 側 | llm-wiki 側で接続 |
|---|---|
| [concepts/Plurality](../concepts/Plurality.md) / [concepts/ブロードリスニング](../concepts/ブロードリスニング.md) / [concepts/広聴AI](../concepts/広聴AI.md) | `concept-as-region` のブロードリスニング向け 6 演算 / [sources/plurality-concept-map](../../../llm-wiki/wiki/sources/plurality-concept-map.md)(Plurality 本の概念マップ、Sensemaker/tttc 比較、KJ 法との緊張) |

## llm-wiki 側にあって、Kozaneba 側で取り込み候補の概念

llm-wiki 側で言語化済みだが、Kozaneba 側の wiki に対応ページが**まだない**重要概念。優先度順:

| 候補概念 | 取り込み価値 |
|---|---|
| **前言語的構造化** | Kozaneba の本質的ニッチを言語化したもの。llm-wiki 側で 3 Wiki 独立収束したと整理されている。Kozaneba 側で concepts/ に独立ページとして持つ価値が高い |
| **構造を仮説として扱う** | Canvas 移行・AI 統合・Clean Relation Elicitation のすべてに通底する設計原則。themes/ に「AI を Kozaneba に入れるなら」(仮)ページを新設するなら核 |
| **関係の平坦化(relation-flattening)** | [themes/関係を第一級にする](../themes/関係を第一級にする.md) の理論的補強として直接効く。TransE/RotatE 等の埋め込み議論や Cosense 比較も含む |
| **概念は領域である(concept-as-region)** | [concepts/N項関係](../concepts/N項関係.md) / [concepts/関係場](../concepts/関係場.md) のポジティブな対案。Gärdenfors 出自 |
| **考える花火** | KJ 法 → 探検ネット → 考える花火 → Keichobot という後期川喜田の系譜が Kozaneba の歴史的位置づけに直結 |
| **identity-without-name** | デライト「知番」のように名前と対象の同一性を分離。こざねの ID 設計、リネーム時の整合性問題に転用可能 |
| **ConnectingDots / Dots / Stories / Views** | Kozaneba を「View の一種」として位置づける枠組み。Kozaneba 単体ではなく上位データモデルから設計を見る視座 |

## 取り込み方針メモ

- llm-wiki 側ページの中身を Kozaneba 側にコピーするのではなく、Kozaneba 側の関連ページの末尾に「llm-wiki 側で関連: [xxx]」セクションを足すのが軽量で安全(リンク密度が上がり、両 wiki の独立性も保たれる)
- 新規 concept ページとして取り込む場合は、Kozaneba 文脈で何を主張するかを優先し、llm-wiki 側の議論は「上位の一般化」として参照する形にする(逆輸入)
- llm-wiki の方が後発(2026-04 以降)で、Kozaneba 側の議論を観察した結果として育っている。同じ素材を扱った場合、llm-wiki 側の方が抽象化が一段進んでいることが多い

## Sources

- [/Users/nishio/llm-wiki/wiki/index.md](../../../llm-wiki/wiki/index.md) — llm-wiki カタログ(全 137 ページ)
- 本ページ作成時点で参照した llm-wiki 側ページ(主なもの):
  - [entities/kozaneba](../../../llm-wiki/wiki/entities/kozaneba.md)
  - [entities/connecting-dots](../../../llm-wiki/wiki/entities/connecting-dots.md)
  - [concepts/pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md)
  - [concepts/kangaeru-hanabi](../../../llm-wiki/wiki/concepts/kangaeru-hanabi.md)
  - [concepts/structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md)
  - [concepts/relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md)
  - [concepts/concept-as-region](../../../llm-wiki/wiki/concepts/concept-as-region.md)
  - [concepts/identity-without-name](../../../llm-wiki/wiki/concepts/identity-without-name.md)
  - [sources/gpt-mindtrellis-connectingdots-20260503](../../../llm-wiki/wiki/sources/gpt-mindtrellis-connectingdots-20260503.md)
