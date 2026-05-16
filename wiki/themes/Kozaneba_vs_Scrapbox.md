---
title: Kozaneba と Scrapbox の関係
type: theme
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
  - raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md
  - raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md
  - raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md
---

## 構造の対応(2023-11 時点の最新理解)

[ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md) と [Scrapboxの行とKozanebaのこざねの対応づけ](../../raw/scrapbox_kozaneba/2023-03-31__Scrapboxの行とKozanebaのこざねの対応づけ.md):

| 概念 | [Scrapbox](../entities/Scrapbox.md) | [Kozaneba](../entities/Kozaneba.md) |
|---|---|---|
| 単位 | 行(line) | [こざね](../concepts/こざね.md) |
| 単位の集まり | ページ(タイトル付き) | グループ(タイトル付き) |
| 単位間の「近さ」 | ベクトル検索/箇条書きの親子 | 空間近接配置 |
| 単位間の「リンク」 | ブラケティングによるリンク | [線を引く機能](../concepts/線を引く機能.md) |
| 圧縮 ⇔ 展開 | タイトル ⇔ ページ本文(両方見える) | グループ閉 ⇔ 開(どちらかしか見えない) |
| 非存在の連想 | 赤リンク([連想的雰囲気](../concepts/連想的雰囲気.md)) | なし |

## 連携の試み(時系列)

### 2021-08-31: Scrapbox URL → Scrapboxこざね

[KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md):

- Scrapbox URL を貼ると [Scrapboxこざね](../concepts/Scrapboxこざね.md) ができる
- expand メニューで 2hop リンクを引き込める
- 用途: 「KJ法 のページが 97 枚もリンクされていて Scrapbox 上では収拾がつかない」状態を Kozaneba で整理

### 2021-12-11: 場をまたぐ思い出し効果

[KozanebaにScrapbox的な思い出し効果をつける](../../raw/scrapbox_kozaneba/2021-12-11__KozanebaにScrapbox的な思い出し効果をつける.md):

- Kozaneba の場が重くなるので分けたい
- 場をまたぐ [RELEVANCE](../concepts/体験過程.md) の発見を支援したい
- Scrapbox のブラケティング = [非平行的シンボル](../concepts/平行的シンボル.md) の表明
- Kozaneba での対応物 = 拡大したこざね、タイトル付きグループ
- これらを場を跨いで繋げる、という構想([思い出し効果](../concepts/思い出し効果.md))

### 2022-03: Scrapbox をインフラ、Kozaneba をオーバーレイ

[Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md):

> 未踏会議 2022 の村井純先生の話。「インターネットは電話線に対するオーバーレイネットワークとして生まれた、有用性が広く知られて使われるようになってから下部レイヤーが交換された」。Scrapbox をインフラとし、それに対するオーバーレイとして Kozaneba を使えるようにする。

実装: マップの「Scrapbox プロジェクト名」フィールド、Scrapbox 記法のパース(アイコン・画像)。Scrapbox 記法のリンクを実リンクにすると操作しにくくなったので不採用。

### 2023-02: プロジェクト全体のインポート → 毛玉問題

[ScrapboxプロジェクトをKozanebaにインポートする実験](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする実験.md): 99 ページ 400 リンクのプロジェクトをインポート → [毛玉問題](../concepts/毛玉問題.md) に直面。「KJ法 の逆 = リンクが多すぎて全体を見ることができない」状態。リンクを切る支援が必要かもしれない。

### 2023-08: 時間的スキーム vs コンテキスト的スキーム

[🤖Kozaneba](../../raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md):

> Scrapbox の行が近接 = こざねが近接 → これは時間的スキーム。時間的スキームとコンテキスト的スキームを自由に行き来できるのが理想。

### 2024-03: A型 = Kozaneba / B型 = Scrapbox 構想

[KozanebaとScrapboxで理解と再利用性を向上](../../raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md):

> 当初の予定は紙でやるのに比べて再利用しやすくすることだった、しかしあまり再利用していない。川喜田二郎本人も [B型文章化](../concepts/B型文章化.md) が必要だと言っている。現状の Kozaneba は図解の部分だけ。文章をペアにして持つことが必要か?

→ **Kozaneba = A型図解 / Scrapbox = B型文章** で運用するアプローチ。Scrapbox のリンク機能・タグで関連情報へのアクセスが容易になる。

## 残された課題(2024-03 時点)

- 図解単独では理解しにくい / 再利用しにくい
- Scrapbox のページ数が増えてから来た人が何をみたらいいかわからない問題(全 Scrapbox を統合したマップが作られていない)([Kozanebaを累積KJ法に近づける](../../raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md))
- Scrapbox こざねが通常こざねと操作上の対等性を持っていない(clone, leave from lines などのメニューが欠落)
- [毛玉問題](../concepts/毛玉問題.md) の根本的解決手段なし

## Sources

- [KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md)
- [KozanebaにScrapbox的な思い出し効果をつける](../../raw/scrapbox_kozaneba/2021-12-11__KozanebaにScrapbox的な思い出し効果をつける.md)
- [Kozaneba:Scrapboxベストプラクティス2022](../../raw/scrapbox_kozaneba/2022-03-02__Kozaneba_Scrapboxベストプラクティス2022.md)
- [Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md)
- [ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする%28開発%29.md)
- [ScrapboxプロジェクトをKozanebaにインポートする実験](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする実験.md)
- [Scrapboxの行とKozanebaのこざねの対応づけ](../../raw/scrapbox_kozaneba/2023-03-31__Scrapboxの行とKozanebaのこざねの対応づけ.md)
- [🤖Kozaneba](../../raw/scrapbox_kozaneba/2023-08-27__🤖Kozaneba.md)
- [ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md)
- [KozanebaとScrapboxで理解と再利用性を向上](../../raw/scrapbox_kozaneba/2024-03-02__KozanebaとScrapboxで理解と再利用性を向上.md)
