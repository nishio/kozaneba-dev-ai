---
title: Kozaneba と Keichobot の関係
type: theme
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-12-22__Kozaneba_Keichobotの文脈を整理したい.md
  - raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md
  - raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md
  - raw/scrapbox_kozaneba/2023-03-04__KozanebaとKeichobotの関係は？.md
  - raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md
  - raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md
---

## コアの分業(2021-12)

[Keichobotは言語化しKozanebaは一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md):

| 局面 | ツール |
|---|---|
| モヤモヤ → 言葉(シンボル) | [Keichobot](../entities/Keichobot.md) |
| シンボル間の関係 → 一次元的なストーリー | [Kozaneba](../entities/Kozaneba.md) |

[KJ法](../concepts/KJ法.md) は両局面にまたがる:

- 「なにか関係ありそうだから近くに置く」 = まだ言語化されてない「関係」を脳の外に出す(Keichobot 寄り)
- 「表札をつけて束ねる」 = 事後的に関係を言葉にする(Keichobot)
- 配置と[線を引く機能](../concepts/線を引く機能.md) = 関係の客体化と[一次元化](../concepts/一次元化.md)(Kozaneba)

## 融合の試み(2021-12〜)

### 2021-12-22: Keichobot の対話ログを Kozaneba で整理

[Kozaneba:Keichobotの文脈を整理したい](../../raw/scrapbox_kozaneba/2021-12-22__Kozaneba_Keichobotの文脈を整理したい.md): Keichobot のチャットログを Kozaneba に取り込んで文脈を整理する試み(画像のみ、テキスト記録は乏しい)。

### 2022-08-19: 探検ネット + 質問キーワードの自動エッジ

[Kozaneba2022-08-19](../../raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md):

> Keichobot の質問キーワードと回答キーワードのペアをエッジでつないでいくと「Aって何?」「B」「Bって何?」「C」というタイプのダメな会話を検知して、良い方向にするためのアドバイスを出せそう。

着想のきっかけ: 「[面白い]の探検ネット」を見ていて「まず一塊の密なネットを作ることを目指すわけだ」と気づいた。KJ法 → [探検ネット](../concepts/探検ネット.md) → Kozaneba の「線」機能、と同じパイプラインで、探検ネット → Keichobot にも繋がる。

> ずっと課題だった KeichobotとKozanebaの融合 の糸口が見えてきたかも。

### 2023-08-27: 時間的 vs コンテキスト的スキーム

[🤖Kozaneba](../../raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md):

> Kozaneba に対する付箋の追加タイミングによって各付箋は暗黙の「時間軸上の位置」を持っている(刻み元の文章の一次元的な構造やチャットの時系列に対応)。これをトピック指向のコンテキスト的スキームに変えていくことが必要だが、それは暗黙に[既存の構造の破壊](../concepts/既存の構造の破壊.md)を伴う。それを保持することによって安心して実行できるようにする。

Keichobot の出力(チャット時系列)を Kozaneba に流し込んだとき、時間順の配置 → トピック順の配置への変換が「ねりねり」になる。

### 2025-11: いどばた経由のパイプライン

[いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md):

> いどばたは AI とのチャットによって言語化されていないものを引き出す仕組み。LLM 以前に西尾がやってた Keichobot と親和性が高い。先週いどばたのシステム上で Keichobot 的コーチングを動かすところまでやった。
>
> Keichobot 的コーチングは **概念とその関係を抽出する** ので、[Plurality](../concepts/Plurality.md) 本の概念マップと関連深い。概念マップの研究を進める上でグラフを単に可視化するだけではなく操作したい、これは今まで西尾は Kozaneba でやってきたこと。ただ、[辺ラベル](../concepts/辺ラベル.md) の機能がないのが今のニーズに不足。

→ パイプライン: いどばた/Keichobot → 概念マップ → Kozaneba/Miro。最終的に Miro に乗り換える可能性も視野に入っている。

### 2026-05-16: Plan A による分業の解体

[3 Plan 議論](3plan議論.md) で、Keichobot / Kozaneba / [いどばた](../entities/いどばた.md) の **3 つを参考にしつつ全く新しいサービス**(= Plan A)を作る方向が表明された。これは 2021-12 以来の「Keichobot は言語化 / Kozaneba は一次元化」の二項分業を、**連続体として一つのシステムに統合する**方向への解体になる。

[Clean Relation Elicitation](Clean_Relation_Elicitation.md) で示された「Situation → Aspect → Relation Field → Projection」のパイプラインが、Plan A の対話レイヤの出発点として最も明確。Keichobot 単体 / Kozaneba 単体ではなく **連続体としての 1 システム**が Plan A の輪郭。

ただし Plan A は新規サービスで、Keichobot / Kozaneba / いどばた の 3 つを deprecate するのか並列に保つのかは未決(cf. [3 Plan 議論](3plan議論.md))。

## 残された課題

- Keichobot の対話ログから自動的に Kozaneba のグラフを生成する機能(2025 年時点で未実装)
- [辺ラベル](../concepts/辺ラベル.md) 機能(技術的にはエッジへのイベントリスナ追加が必要)
- 概念マップを操作する UI(Miro でやるか Kozaneba 拡張か)
- 上記 3 つは Plan B 期間内で部分的に着手するか、Plan A で全部書き直すかの判断保留中

## Sources

- [Kozaneba:Keichobotの文脈を整理したい](../../raw/scrapbox_kozaneba/2021-12-22__Kozaneba_Keichobotの文脈を整理したい.md)
- [Keichobotは言語化しKozanebaは一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md)
- [Kozaneba2022-08-19](../../raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md)
- [KozanebaとKeichobotの関係は？](../../raw/scrapbox_kozaneba/2023-03-04__KozanebaとKeichobotの関係は？.md)
- [🤖Kozaneba](../../raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md)
- [いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md)
