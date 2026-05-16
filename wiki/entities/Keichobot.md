---
title: Keichobot
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md
  - raw/scrapbox_kozaneba/2021-12-22__Kozaneba_Keichobotの文脈を整理したい.md
  - raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md
  - raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md
  - raw/scrapbox_kozaneba/2023-03-04__KozanebaとKeichobotの関係は？.md
  - raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md
---

## 定義

**Keichobot**(けいちょぼっと、`nisbot` とも呼ばれる)は nishio が作った「あなたの話を聞いてくれるチャットボット」。クリーンランゲージ(CL)の質問体系をベースにしている。URL: `https://keicho.netlify.app/`

## [Kozaneba](Kozaneba.md) との関係

[Keichobotは言語化しKozanebaは一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md) が両者の役割分担を明示する基本文書:

> Keichobot はまだ言葉になってないモヤモヤを言葉(シンボル)にすることを促し / Kozaneba は関係を客体化し、それを一次元的なストーリーにすることを支援する。

つまり Keichobot は **モヤモヤ→言葉** の局面で、Kozaneba は **関係のネットワーク→[一次元化](../concepts/一次元化.md)されたストーリー** の局面で使われる。両者は補完関係。

詳しくは [テーマ: Kozaneba と Keichobot の融合](../themes/Kozaneba_vs_Keichobot.md)。

## 実例: nishio 自身が Keichobot を使って Kozaneba の方向性を整理

[Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md) は、nishio が Keichobot との対話を通じて「Kozaneba を作る目的が自分の中でブレている」ことを発見し、最終的に「**他の人に使われることによって、より良くなるといい**」という結論に至った記録。

## 融合の試み

- 2022-08-19 [Kozaneba2022-08-19](../../raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md): Keichobot の質問キーワードと回答キーワードのペアをエッジで結ぶことで「Aって何？」「B」「Bって何？」「C」というダメな会話パターンを検知し、アドバイスを出す方向の着想。「探検ネット → Kozaneba」と「探検ネット → Keichobot」がつながり、長年の課題だった「KeichobotとKozanebaの融合」の糸口が見えた、と nishio。
- 2025-10/11 [いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md): LLM 以前から Keichobot がやっていた「言語化されていないものを引き出す」が、いどばた(LLM ベースのチャットシステム)で動くようになり、概念マップを介して Kozaneba(または Miro)に接続するパイプラインが構想されている。Kozaneba 側に [辺ラベル](../concepts/辺ラベル.md) 機能が不足。

## Sources

- [Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)
- [Kozaneba:Keichobotの文脈を整理したい](../../raw/scrapbox_kozaneba/2021-12-22__Kozaneba_Keichobotの文脈を整理したい.md)
- [Keichobotは言語化しKozanebaは一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md)
- [Kozaneba2022-08-19](../../raw/scrapbox_kozaneba/2022-08-23__Kozaneba2022-08-19.md)
- [KozanebaとKeichobotの関係は？](../../raw/scrapbox_kozaneba/2023-03-04__KozanebaとKeichobotの関係は？.md)
- [いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md)
