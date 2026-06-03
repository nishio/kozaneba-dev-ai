---
title: Scrapboxこざね
type: concept
created: 2026-05-16
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2021-08-28__Kozanebaの開発をKozanebaで管理.md
  - raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md
  - raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md
  - raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする(開発).md
  - raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md
  - raw/kozaneba-forum/Release_Notes.md
  - raw/kozaneba-forum-jp/リリースノート.md
  - raw/kozaneba-forum-jp/Scrapbox連携機能.md
  - raw/kozaneba-forum-jp/解決:存在しないScrapboxページのこざねを作るとクラッシュ.md
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

## Scrapbox / 画像連携の時系列(Release Notes より)

[Release Notes 2021-2025 要約](../sources/release-notes-2021-2025.md) で確認した、Scrapbox / 画像周りの主な変遷:

- **2021-08-30**: Scrapbox / Gyazo Kozane 追加 (URL paste で kozane 化、project top page は除外)。expand menu で 2hop 内全ページを kozane 化
- **2022-03-24**: **Scrapbox Integration** 本格化。Ba に project 名を設定すれば kozane テキストを Scrapbox 記法としてパース、アイコンを画像化
- **2022-05-30**: 外部リンク kozane に target domain の **favicon** を付与
- **2022-06-03**: Image URL パターン拡張。`*.png` URL も image kozane に、`[]` 囲み記法、行内に複数 `[]` で複数 image kozane 化。Scrapbox からそのまま貼り付け可能に
- **2022-06-09**: Scrapbox Integration ON 時、Kozane / Group の context menu に「**expand scrapbox links**」を追加 (link 記法を解釈して link 先を全部 import)
- **2023-01-16**: 存在しない Scrapbox page を kozane 化したときの挙動を明示 (`empty` / `Page not found` / `Project not found` の 3 種)

「URL を貼ったら自動でリッチな kozane になる」「貼り付けたテキストが Scrapbox 記法で書かれていたら拾う」という挙動は、**ユーザの『コピペ』動作をそのまま許容する** 設計思想を体現している。これは [Scrapbox](../entities/Scrapbox.md) の「ブラケット書けば link」と思想が近い (両者ともマイクロインタラクション最小化)。

## リンクをクリック可能にしない設計判断(フォーラム解説)

公開フォーラム [Scrapbox Integration](../../raw/kozaneba-forum/Scrapbox_Integration.md) (2022-05-26) で nishio が公開した重要な解説:

> リンクに関しては、そのままリンクにするとこざねをドラッグしようとしてリンクを開いてしまう事故が起きるのではないかと思って避けています。
> 「こざねを動かすこと」が最も頻出の行動で、その行動が「四角の中ならどこでマウスダウンしてもOK」なのと「よく見てリンクのないところでマウスダウンしないといけない」のでは認知的な負荷が大違いだからです。
> 以前、線に当たり判定をつけて設定メニューが出るようにしようとしたことがあって、これも「こざねを動かすこと」の妨げになったのでやめました。

「**こざねを動かす**」を一級操作と位置付け、それを邪魔する可能性のあるクリックハンドラを徹底的に削っている。これは [線を引く機能](線を引く機能.md) の「エッジに pointerEvents: 'none'」コード上の事実の **設計意図**で、辺ラベル UX を考えるときの根本制約。

## 外部ユーザからの crash 報告

2022-09-21 に外部ユーザ `YJ` が [存在しない Scrapbox ページのこざねを作るとクラッシュ](../../raw/kozaneba-forum-jp/解決:存在しないScrapboxページのこざねを作るとクラッシュ.md) を報告。expand で連携先にない単語を Scrapbox 記法でリンク化したときに crash する症状。2023-01-16 のリリースで修正(`Page not found` / `Project not found` を画面で示すように)。

## 課題

[ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする%28開発%29.md):

> Scrapbox こざねに leave from lines メニューがついてない / clone もついてない。

Scrapbox こざねが通常のこざねと「対等な操作対象」になり切れていない問題。

## Sources

- [Kozanebaの開発をKozanebaで管理](../../raw/scrapbox_kozaneba/2021-08-28__Kozanebaの開発をKozanebaで管理.md)
- [KozanebaでScrapboxのリンクを整理](../../raw/scrapbox_kozaneba/2021-08-31__KozanebaでScrapboxのリンクを整理.md)
- [Kozaneba+Scrapbox](../../raw/scrapbox_kozaneba/2022-03-25__Kozaneba+Scrapbox.md)
- [ScrapboxプロジェクトをKozanebaにインポートする(開発)](../../raw/scrapbox_kozaneba/2023-02-17__ScrapboxプロジェクトをKozanebaにインポートする%28開発%29.md)
- [Kozanebaを累積KJ法に近づける](../../raw/scrapbox_kozaneba/2024-03-07__Kozanebaを累積KJ法に近づける.md)
- [raw/kozaneba-forum/Release_Notes.md](../../raw/kozaneba-forum/Release_Notes.md) — 2021-2023 の Scrapbox / 画像連携の時系列(英語版)
- [raw/kozaneba-forum-jp/リリースノート.md](../../raw/kozaneba-forum-jp/リリースノート.md) — 日本語版リリースノート
- [raw/kozaneba-forum-jp/Scrapbox連携機能.md](../../raw/kozaneba-forum-jp/Scrapbox連携機能.md) — リンクをクリック可能にしない設計判断
- [raw/kozaneba-forum-jp/解決:存在しないScrapboxページのこざねを作るとクラッシュ.md](../../raw/kozaneba-forum-jp/解決:存在しないScrapboxページのこざねを作るとクラッシュ.md) — 外部ユーザ YJ からのクラッシュ報告(2022-09-21)
