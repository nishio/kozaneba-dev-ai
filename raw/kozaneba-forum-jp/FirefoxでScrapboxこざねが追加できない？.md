# FirefoxでScrapboxこざねが追加できない？

## メタデータ

- タイトル: FirefoxでScrapboxこざねが追加できない？
- 作成日時: 2021-08-31T12:05+09:00 (5 years ago)
- 最終更新日時: 2021-08-31T12:21+09:00 (5 years ago)
- 最終アクセス日時: 2026-05-28T17:33+09:00 (6 days ago)
- 被リンク数: 0
- pageRank: 0
- views: 27
- 行数: 10
- 文字数: 607
- 作成者: nishio
- 最終更新者: nishio

## 人間のアイコン記法

- [nishio.icon]

## テロメアのサマリー

- nishio	更新期間 2021/8/31 〜 2021/8/31	10行更新

## 本文

FirefoxでScrapboxこざねが追加できない？
>KozanebaでScrapboxページの読み込みテスト。URLをコピーして、Kozaneba上でペースト。無事できた（Firefoxでは無理で、Chromeならできた）。 [src https://twitter.com/rashita2/status/1432509880116539400]
[nishio.icon]ブラウザ機能としてはFirefoxでも動くはずなので、もしかしたら画面をクリックしてからとか何かの条件で動いたりするかもしれません。
	[https://scrapbox.io/files/612d9cf1d3049c001e1f11f4.png]
	https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/onpaste
	ダメそうならスマホ版Scrapboxみたいに「ペーストのためのダイアログを出す」という方法でワークアラウンドします
	[https://scrapbox.io/files/612d9ff98532f7001d9fecaa.png]
	https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/readText
	あー、なるほど、onpasteはあるけどクリップボードの中身を読むことができないのか

