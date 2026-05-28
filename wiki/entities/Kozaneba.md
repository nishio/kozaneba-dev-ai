---
title: Kozaneba
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md
  - raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md
---

## 定義

**Kozaneba** = 「かんがえをまとめるデジタル文房具」。Web アプリ。OSS、無償提供。作者: nishio。

- 公式チュートリアル / 試用: https://kozaneba.netlify.app/
- ステータス: 実験的実装フェーズ(安定性重視ではない)

## モチベーション

KJ法的な手法は有益だが、紙でやると不便がある。よいデジタル文房具が欲しい。nishio 自身がまず使いたいツールとして始まったが、現在は「**他の人に使われて成長すること**」を主目的に据えている。詳しくは [Kozanebaを作ることで何がどうなればいいのか](../themes/なぜ作るのか.md)(未作成)。

## 主要機能

- **グループを畳む** — 大きくなったまとまりを一時的に折りたたんで全体を見渡しやすくする
- **重要なものを大きくする** — サイズで重要度を表現
- **ものの間に関係の線を引く** — ノード間のつながりを明示

## 系譜

- 前身: [Regroup](Regroup.md) → [Movidea](Movidea.md)
- 関連:
  - [Keichobot](Keichobot.md) — 言語化を促す対話ボット。Kozaneba と対比して論じられることが多い(「[Keichobot は言語化し Kozaneba は一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md)」、テーマページ [Kozaneba vs Keichobot](../themes/Kozaneba_vs_Keichobot.md))
  - [Scrapbox](Scrapbox.md) — リンクベースのノートツール。Kozaneba とは構造的に対比される(「[ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md)」、テーマページ [Kozaneba vs Scrapbox](../themes/Kozaneba_vs_Scrapbox.md))

## 2026-05-16: 現状(Plan B)と次世代(Plan A)の二段構え

[3 Plan 議論](../themes/3plan議論.md) で、Kozaneba の今後について **両方を並行する** が選ばれた:

- **現 Kozaneba(Plan B 改造の対象)**: nishio 自身の 300 件作業の加速、[源の長文](../concepts/源の長文.md) データモデル拡張、現 React/DOM 実装の継続。Canvas 化([Canvas移行の検討](../themes/Canvas移行の検討.md))は急がない方向
- **次世代(Plan A、新規サービス)**: [Keichobot](Keichobot.md) / [いどばた](いどばた.md) / Kozaneba の 3 つを参考にした **全く新しいサービス**。データモデルから新規

Plan A は Kozaneba の直接の継承ではなく、Kozaneba を deprecate するか並列に保つかは未決。Plan B 期間中に Plan A の要求仕様を貯める方針(現 Kozaneba を使いながら「やりたかったができなかった」操作・AI と対話したかった場面・関係を引きたかったが諦めた瞬間 etc を記録)。

## このリポジトリでの分量

`raw/scrapbox_kozaneba/` に 403 ページ(タイトルに Kozaneba を含むものが 117 ページ)。最初のメンション 2017-09-03、最新 2026-05-16。年別の分布:

| 年 | ページ数 |
|---|---|
| 2017 | 1 |
| 2018 | 1 |
| 2019 | 1 |
| 2020 | 2 |
| 2021 | 102 |
| 2022 | 86 |
| 2023 | 131 |
| 2024 | 35 |
| 2025 | 36 |
| 2026 | 8 |

開発・思索のピークは 2021〜2023。

## Sources

- [かんがえをまとめるデジタル文房具Kozaneba](../../raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md)
- [Kozaneba](../../raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md)
