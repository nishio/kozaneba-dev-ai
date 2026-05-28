---
title: Canvas 実装への移行検討
type: theme
created: 2026-05-16
updated: 2026-05-25
sources:
  - raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md
  - raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md
  - raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md
  - work/kozaneba
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

## 2026-05-16: 「両方やる」への着地

[3 Plan 議論](3plan議論.md) で、上記「分けるか統合するか」の問いに **両方やる** で着地した:

- **Plan B**(現 Kozaneba 改造): nishio 自身の 300 件作業を加速するためのドッグフーディング駆動修正。[源の長文](../concepts/源の長文.md) データモデル拡張を含む。Canvas 化 vs 現 DOM のままで済ますかは Plan B 期間で再評価
- **Plan A**(新規サービス): [Keichobot](../entities/Keichobot.md) / [いどばた](../entities/いどばた.md) / Kozaneba を参考にした全く新しいサービス。データモデルから新規。Kozaneba を deprecate するか並列にするかは未決

Canvas 移行論はもともと「Kozaneba の延長線上で大規模化どうする」の問いだったが、Plan A の登場で問いの形が変わる: 大規模化が必要なのは現 Kozaneba ではなく Plan A 側かもしれず、現 Kozaneba の Canvas 化を急ぐ理由は弱まる。

## 2026-05-19: Canvas は「答え」ではなく 3 案のうちどこで必要かを見極める対象

[3つのストーリー比較](3つのストーリー比較.md) の整理を入れると、Canvas 化は単独の目標ではなくなる。

- **改善ストーリー**では、Canvas は不要かもしれない。300 件規模の作業が DOM 改善で十分速くなるなら、急ぐ理由はない
- **似たものを新しく作るストーリー**では、Canvas は候補の 1 つ。ただし本質は描画エンジンより Situation / Relation / View のモデル再設計
- **全く新しいものを作るストーリー**では、入口が対話や読書になる可能性が高く、Canvas は主役ではなく「後から見る view」の 1 つに下がる可能性がある

したがって「Canvas にするか」は最上位の分岐ではなく、どのストーリーに進むかが先に来る判断になる。

## 2026-05-25: コード一次調査で見えた DOM 実装のボトルネック構造

[work/kozaneba コード構造調査](../sources/kozaneba-code-architecture.md) で確認した実装事実から、Canvas 化議論を精密化:

- DOM レイヤ自体だけでなく、**`Physics/ItemRepulse.ts` が全 Item ペア走査の O(N²)** であり、これは描画エンジンを変えても残る。物理を使う場合、quadtree / Barnes-Hut 化が並行課題
- 状態管理は `reactn` の単一グローバル state で、`setGlobal` が全 `useGlobal` を再評価しうる。`React.memo` + `useMemo` + `useCallback` で 2025-04 に最適化されたが([git history 2025](../sources/kozaneba-git-history-2025.md))、大規模化では state shape の分割か別の state ライブラリへの差し替えが先に効く可能性がある
- 注釈レイヤは SVG で、`AnnotationLayer.tsx` が `pointerEvents: "none"` で全イベントを背景に通す設計。Canvas 化するときに「線をクリックして編集」を成立させるなら、ヒットテストを自前で書く必要があり、これは Devin が WebGL を避けた理由(テキスト描画と並ぶ再実装コスト)と同じ性質の負担

つまり Canvas 化を仮にやるとして、**書き直さなければならないのは描画だけでなく、(a) 物理アルゴリズム、(b) ヒットテスト、(c) 状態管理のスコープ** の 3 つが連動する。Devin の「WebGL 推奨しない」判断はこの 3 点コストを暗黙に評価したものとして読める。

## Sources

- [Kozanebaのコードを丸ごとo1 Proに入れる](../../raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md)
- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md)
- [pKozaneba2025-08-26~27](../../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md)
- [pKozaneba2025-08-29](../../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md)
