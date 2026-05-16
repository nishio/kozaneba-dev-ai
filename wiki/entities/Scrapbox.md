---
title: Scrapbox
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md
  - raw/scrapbox_kozaneba/2021-12-11__KozanebaにScrapbox的な思い出し効果をつける.md
  - raw/scrapbox_kozaneba/2022-03-02__Kozaneba_Scrapboxベストプラクティス2022.md
  - raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md
  - raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md
  - raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする実験.md
  - raw/scrapbox_kozaneba/2023-03-31__Scrapboxの行とKozanebaのこざねの対応づけ.md
  - raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md
  - raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md
---

## 定義

**Scrapbox** は Helpfeel(旧 Nota)が提供するリンクベースの集合知ノートツール。nishio が長年ヘビーに使っており、`nishio.json` だけで 25,633 ページの蓄積がある(2026-05 時点)。Kozaneba と並んで nishio の知的生産の中核ツール。

## [Kozaneba](Kozaneba.md) との構造の比較

[ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md) と [Scrapboxの行とKozanebaのこざねの対応づけ](../../raw/scrapbox_kozaneba/2023-03-31__Scrapboxの行とKozanebaのこざねの対応づけ.md) によれば:

| Scrapbox | Kozaneba |
|---|---|
| ページ間のリンク | こざね間の [線](../concepts/線を引く機能.md) |
| ベクトル検索による「近さ」 | こざねの近接配置 |
| 行(line) | [こざね](../concepts/こざね.md) |
| ページ(=行の集まり+タイトル) | グループ(=こざねの集まり+タイトル) |
| 「圧縮(タイトル)」と「展開(ページ本文)」のリンク | グループの開閉(どちらか一方しか見えない) |
| 赤リンク(=非存在ページ) | 非存在こざねという概念がない |

赤リンクは「具体的なページに到達していない[連想的雰囲気](../concepts/連想的雰囲気.md)」の外在化に成功している。

## 連携の試み(統合の歴史)

詳細は [テーマ: Kozaneba と Scrapbox の連携](../themes/Kozaneba_vs_Scrapbox.md)。

- 2021-08-31 [KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md): Scrapbox の URL を貼ると **Scrapboxこざね** ができて、`expand` メニューで 2hop リンクをすべて引き込める機能を作った
- 2021-12-11 [KozanebaにScrapbox的な思い出し効果をつける](../../raw/scrapbox_kozaneba/2021-12-11__KozanebaにScrapbox的な思い出し効果をつける.md): 場を跨いだ [RELEVANCE](../concepts/体験過程.md) の発見を支援する構想([思い出し効果](../concepts/思い出し効果.md))
- 2022-03-25 [Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md): 「Scrapbox をインフラ、Kozaneba をオーバーレイ」として位置付ける方針を採用。マップに Scrapbox プロジェクト名のフィールドを追加、Scrapbox 記法のパース(アイコン・画像)を実装
- 2023-02-17 [ScrapboxプロジェクトをKozanebaにインポートする実験](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする実験.md): 99 ページ 400 リンクの可視化に挑戦したが、[毛玉問題](../concepts/毛玉問題.md) に直面
- 2024-03-02 [KozanebaとScrapboxで理解と再利用性を向上](../../raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md): 図解 = Kozaneba / 文章 = Scrapbox のペアで [B型文章化](../concepts/B型文章化.md) する構想

## Sources

- [KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md)
- [KozanebaにScrapbox的な思い出し効果をつける](../../raw/scrapbox_kozaneba/2021-12-11__KozanebaにScrapbox的な思い出し効果をつける.md)
- [Kozaneba:Scrapboxベストプラクティス2022](../../raw/scrapbox_kozaneba/2022-03-02__Kozaneba_Scrapboxベストプラクティス2022.md)
- [Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md)
- [ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md)
- [ScrapboxプロジェクトをKozanebaにインポートする実験](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする実験.md)
- [Scrapboxの行とKozanebaのこざねの対応づけ](../../raw/scrapbox_kozaneba/2023-03-31__Scrapboxの行とKozanebaのこざねの対応づけ.md)
- [ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md)
- [KozanebaとScrapboxで理解と再利用性を向上](../../raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md)
