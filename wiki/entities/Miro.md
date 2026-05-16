---
title: Miro
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2022-06-27__探検ネット(花火)勉強会.md
  - raw/scrapbox_kozaneba/2023-01-20__線を引くUI.md
  - raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md
---

## 定義

**Miro** はオンラインホワイトボード SaaS。付箋・図形・線の自由配置、リアルタイム共同編集、テンプレート、Zoom 連携などを備える商用ツール。[Kozaneba](Kozaneba.md) が個人開発の OSS なのに対し、Miro は **既に大規模に普及している商用プロダクト**。

## Kozaneba にとっての位置づけ

Miro は Kozaneba の **代替候補**・**競合**・**参考実装** の三つの位置づけを持つ。

### 代替候補(2025-11)

[いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md):

> Kozaneba に [辺ラベル](../concepts/辺ラベル.md) の機能をつける方向で調査していて、**Miro でもいいのではという案** が出てきたところ。

[Plurality](../concepts/Plurality.md) 本の概念マップ操作のために辺ラベルが必要になり、Kozaneba を拡張するか、Miro に乗り換えるかという判断点に至っている。詳細は [Canvas移行の検討](../themes/Canvas移行の検討.md)。

### 競合・参考実装

- [線を引くUI](../../raw/scrapbox_kozaneba/2023-01-20__線を引くUI.md): Miro の線描画 UI を比較対象として参照
- KJ法 勉強会・[探検ネット](../concepts/探検ネット.md) 勉強会では Miro が「2022 年時点のデジタル KJ法 環境のデファクト」として参照される([探検ネット(花火)勉強会](../../raw/scrapbox_kozaneba/2022-06-27__探検ネット%28花火%29勉強会.md))

## Kozaneba との根本的な違い

ソースを横断すると以下が観察される:

- **思想**: Kozaneba は KJ法/こざね法 という具体的方法論を支援する思想駆動、Miro は汎用ホワイトボード
- **開放性**: Kozaneba は OSS・無料、Miro は商用 SaaS
- **規模**: Miro はビジネスユーザに広く普及、Kozaneba は個人開発で利用者が限られる
- **思い出し効果**: Kozaneba は [Scrapbox](Scrapbox.md) 連携や [思い出し効果](../concepts/思い出し効果.md) など独自の研究テーマがある

## 関連

- [Kozaneba](Kozaneba.md)
- [Canvas移行の検討](../themes/Canvas移行の検討.md) — Miro 乗り換えを含む議論
- [辺ラベル](../concepts/辺ラベル.md) — Miro が持ち Kozaneba が持たない機能

## Sources

- [探検ネット(花火)勉強会](../../raw/scrapbox_kozaneba/2022-06-27__探検ネット%28花火%29勉強会.md)
- [線を引くUI](../../raw/scrapbox_kozaneba/2023-01-20__線を引くUI.md)
- [いどばた/Keichobot→概念マップ→Kozaneba/Miro](../../raw/scrapbox_kozaneba/2025-11-14__いどばた_Keichobot→概念マップ→Kozaneba_Miro.md)
