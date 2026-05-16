---
title: Scrapboxこざね
type: concept
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-28__Kozanebaの開発をKozanebaで管理.md
  - raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md
  - raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md
  - raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md
  - raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md
---

## 定義

**Scrapboxこざね** は [Kozaneba](../entities/Kozaneba.md) 上のこざねの一種で、[Scrapbox](../entities/Scrapbox.md) のページを表現するもの。URL を貼ると自動生成される。

## 機能(2021-08 時点)

[KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md):

> Kozaneba に Scrapbox の URL を貼ると Scrapbox こざねができて、それの expand メニューを押すと 2hop 以内のすべてのページがこざねになり、自由に動かせる。

これは「Scrapbox は階層構造を作ることを支援しない」「ページ内の行は箇条書きで 1.5 次元で動かせるが、ページのカード表示は動かせない」という Scrapbox の弱点を Kozaneba で補う発想。

## サムネイル化(2021-08-28)

[Kozanebaの開発をKozanebaで管理](../../raw/scrapbox_kozaneba/2021-08-28__Kozanebaの開発をKozanebaで管理.md):

> Scrapbox こざねを作れるか考え出すのが面白さムーブで、「意外と Gyazo をそのまま読めるのでは?」と気づいてからが仮説が正しいか検証する不確実性削減ムーブ。

実装プロセスは「[合理的ムーブ / 楽さドリブン / 不確実性削減ムーブ / 面白さドリブン]」の 4 パターンが交錯。

## Scrapbox 連携の進化

[Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md):

- マップに「Scrapbox のプロジェクト名」フィールドを追加
- 空文字列でなければ、こざねのテキストを Scrapbox 記法としてパース
- アイコン記法はアイコンとして埋め込み
- 画像記法は画像こざねに
- Scrapbox 記法のリンクをそのままリンクにすると操作しにくくなるので最終的にはやめた

## [累積KJ法](累積KJ法.md) との関係

[Kozanebaを累積KJ法に近づける](../../raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md):

> 累積KJ法 ではデータバンクは Scrapbox 的(各ラベルの隅にデータカードの ID があり、文章化の時にそれを参照)。これは Kozaneba でいうところの Scrapbox こざね。**より詳細な情報へのリンクを保ったカード**。Kozaneba はこれを今なんとなく「Scrapbox カードの見た目」にしてるけど好ましくないかも、テキストが自由に編集できるべき。

## 課題

[ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md):

> Scrapbox こざねに leave from lines メニューがついてない / clone もついてない。

Scrapbox こざねが通常のこざねと「対等な操作対象」になり切れていない問題。

## Sources

- [Kozanebaの開発をKozanebaで管理](../../raw/scrapbox_kozaneba/2021-08-28__Kozanebaの開発をKozanebaで管理.md)
- [KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md)
- [Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md)
- [ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md)
- [Kozanebaを累積KJ法に近づける](../../raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md)
