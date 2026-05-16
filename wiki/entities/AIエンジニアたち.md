---
title: AI エンジニアたち
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2024-12-08__o1_Proに「LLMを使いこなすエンジニアの知的生産術」の前書きと1章を読ませてみた.md
  - raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-09-11.md
---

## このページについて

Kozaneba 開発の 2024-12 以降の **複数の AI モデル/エージェントを横断的に整理** したエントリ。個別の [Devin](Devin.md) には別ページがある。

## 顔ぶれ

### OpenAI o1 Pro(2024-12〜)

- 2024-12 に nishio がコードを丸ごと o1 Pro に投げた。[Kozanebaのコードを丸ごとo1 Proに入れる](../../raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md)
- 「アプリは『こざね』と呼ばれるテキスト片を『場(Ba)』上に並べ、グルーピングや再配置などによって思考整理を行うツールです」と全体像を要約させ、潜在バグ・改善点を指摘させた
- 指摘内容: UserScript の eval 使用のセキュリティリスク、Firestore 未検出エラー処理、イベントリスナー解除、巨大な状態のレンダリングコスト、Zoom/スクロール時の負荷、スタイル分離、クリップボードコピーのエラー処理
- 並行して [Quartz のコードをまるごと o1 Pro に入れる] も実施(関連ページ)

### GPT-5(2025-09〜)

- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md): [Devin](Devin.md) が WebGL を避けたがるので GPT-5 にセカンドオピニオンを求めた。GPT-5 は「20000 枚でも十分なパフォーマンス」という Devin の主張を「嘘だろ」と詰めた

### Claude(2025-)

- pKozaneba 関連ページに名前が頻出。具体的にどのバージョンの Claude かは記録によって異なる
- Devin と並ぶ AI エンジニアのオプションとして扱われている

### [Devin](Devin.md)(2025-03〜)

- Cognition 社の自律型 AI ソフトウェアエンジニア
- 大規模リファクタを避けたがる傾向あり
- 個別ページに詳細

## Kozaneba 開発体制の変化

| 期 | 体制 |
|---|---|
| 2019〜2024 前半 | nishio 単独開発、断続的 |
| 2024-12 | o1 Pro による全体レビュー、潜在バグ洗い出し |
| 2025-03〜 | [Devin](Devin.md) を本格投入、PR ベースの開発 |
| 2025-08〜 | Devin + GPT-5 セカンドオピニオン、複数 AI のクロスチェック |

これは [Canvas移行の検討](../themes/Canvas移行の検討.md) の議論を加速させた背景の一つ。

## 関連

- [Devin](Devin.md) — 個別の詳細ページ
- [広聴AI](../concepts/広聴AI.md) — Devin が雑な指示でコケた話あり
- [Canvas移行の検討](../themes/Canvas移行の検討.md)

## Sources

- [o1 Proに「LLMを使いこなすエンジニアの知的生産術」の前書きと1章を読ませてみた](../../raw/scrapbox_kozaneba/2024-12-08__o1_Proに「LLMを使いこなすエンジニアの知的生産術」の前書きと1章を読ませてみた.md)
- [Kozanebaのコードを丸ごとo1 Proに入れる](../../raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md)
- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md)
- [pKozaneba2025-09-11](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-09-11.md)
