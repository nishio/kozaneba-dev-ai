---
title: Canvas 実装への移行検討
type: theme
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md
  - raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md
  - raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md
---

## 背景

[Kozaneba](../entities/Kozaneba.md) は現在 React + DOM ベースの実装。2025-08 ごろから Canvas 実装(WebGL や WebGPU を含む)への移行を [Devin](../entities/Devin.md) や GPT-5 とともに検討している。

## 現状のボトルネック

[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md):

- **データサイズ**: 1 つの JSON で Firebase に保存しており、約 2000 枚で限界
- **可読性**: 1000 枚を超えるとテキストが豆粒になる
- 単純にサイズ上限をクリアして描画エンジンを変えて 10000 件表示しても、ユーザ価値は低い

> 広聴 AI 的なもののリーフノードを単なる点ではなく付箋にすることを考えてる。

## Devin による技術評価

[pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md):

| 技術 | Devin の評価 |
|---|---|
| WebGPU | 2025 年現在も互換性低い、大規模な書き直しが必要、推奨しない |
| WebGL | 96.57% のブラウザで対応、性能は期待できるが開発コスト非常に高い。テキスト描画やアクセシビリティ対応、Scrapbox/Gyazo 統合の再実装が大変。現在の `fast_drag_manager.ts` などで最適化済みで明確なパフォーマンス問題はない。**WebGL 移行は推奨しない**。Canvas 2D API 導入や仮想化、WebWorker 活用での改善を |
| Canvas (konva.js) | GPT-5 は「20000 枚で十分なパフォーマンス」と主張 → Devin に「嘘だろ」と詰められる |

新規プロジェクトでも Devin は WebGL を避けたがる傾向([pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md))。

## プロトタイプの実装(2025-08-26~27)

[pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md) で nishio + GPT-5/Claude が Canvas プロトタイプを実装:

- 1 万枚付箋を出して性能上問題なくズームできることを確認
- UMAP で 2 次元に埋め込んでから付箋として配置
- **格子点へのスナップ**(NOTE_SIZE=120px で割る整数格子) → 各格子点の付箋数をカウント、密度を可視化
- **重複時に螺旋状に空きグリッドを探す配置ロジック**

デプロイ: `https://canvas-kozaneba-prototype.vercel.app/`

> 四角い塊になっているところは基本的に座標を維持する散布図では一点に潰れてしまっているデータ。**密度の高さを大きさに変換して可視化** できている。

### 「密度の高さを大きさに変換」vs Kernel Density Estimation

- 散布図の点は「1 つ 1 つが意見」と一般人が認識しにくい → 付箋形式の方がワークショップ経験者に伝わる
- Kernel Density Estimation で等高線で書くのはデータサイエンティスト向け。一般人の認知には抽象度が高すぎる

## 2 つの Kozaneba

[pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md):

> やっぱ 10000 件以上のものをどうするかという路線と、1000 件未満の付箋を作りながら構造化していく Kozaneba は無闇に同一視しない方が良い気がするなぁ。

→ **大規模(広聴 AI 系、UMAP 配置)** と **小規模(KJ法 / こざね法的、人間が構造化)** は別物として扱うべき。

## 残された判断

- 既存の Kozaneba を Canvas 化するか(大規模書き直し)
- 新規プロジェクトとして Canvas 版を別途作るか
- 「広聴 AI のリーフノード = 付箋」のニーズと Kozaneba を本当にマージすべきか

## Sources

- [Kozanebaのコードを丸ごとo1 Proに入れる](../../raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md)
- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md)
- [pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)
- [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)
