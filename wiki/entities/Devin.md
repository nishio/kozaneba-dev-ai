---
title: Devin
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2025-03-11__pKozaneba2025-03-11.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md
  - raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-09-11.md
  - raw/scrapbox_kozaneba/2025-09-14__Kozanebaにエッジラベルをつけると良いのでは.md
---

## 定義

**Devin** は Cognition 社の自律型 AI ソフトウェアエンジニア(`Devin.ai`)。2025 年に nishio が Kozaneba の改善作業に投入している。

## Kozaneba 開発での使用例

- 2025-03-11 [pKozaneba2025-03-11](../../raw/scrapbox_kozaneba/2025-03-11__pKozaneba2025-03-11.md): 初投入。`DEVIN_MEMO.md` を作らせ、ローカル環境でアプリを起動させ、READMEまで充実させた。「すごいぞ Devin」と nishio
- 2025-08-14 [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md): WebGPU / WebGL 移行の是非を相談。Devin は「現時点では推奨しない」と回答。新規プロジェクトでも WebGL を避けたがるので GPT-5 にセカンドオピニオンを求める展開に
- 2025-09-11 [pKozaneba2025-09-11](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-09-11.md): Merge 挙動の変更、`Scale Double` メニュー追加、Selection メニュー拡張などを PR(#33-#35)
- 2025-09-14 [Kozanebaにエッジラベルをつけると良いのでは](../../raw/scrapbox_kozaneba/2025-09-14__Kozanebaにエッジラベルをつけると良いのでは.md): 1 回のフィードバックで「データ構造へのフィールド追加」「ビューへの表示追加」「作成時ダイアログ追加」を全部やってくれたのを「すごい」と評価。一方、ダイアログはウザいので不採用

## 観察された傾向

- 大規模なリファクタを避けたがる傾向(WebGPU/WebGL 移行のケース)
- 仕様の入力が不十分でも自律的に補って実装してしまうことがあり、結果として一部不採用になる(エッジラベルダイアログ)

## Sources

- [pKozaneba2025-03-11](../../raw/scrapbox_kozaneba/2025-03-11__pKozaneba2025-03-11.md)
- [pKozaneba2025-08-14](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-08-14.md)
- [pKozaneba2025-09-11](../../raw/scrapbox_kozaneba/2025-09-11__pKozaneba2025-09-11.md)
- [Kozanebaにエッジラベルをつけると良いのでは](../../raw/scrapbox_kozaneba/2025-09-14__Kozanebaにエッジラベルをつけると良いのでは.md)
