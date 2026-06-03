# Scrapbox Integration

## メタデータ

- タイトル: Scrapbox Integration
- 作成日時: 2022-05-26T17:46+09:00 (4 years ago)
- 最終更新日時: 2022-05-26T17:48+09:00 (4 years ago)
- 最終アクセス日時: 2026-04-19T04:13+09:00 (2 months ago)
- 被リンク数: 1
- pageRank: 1
- views: 26
- 行数: 19
- 文字数: 1116
- 作成者: nishio
- 最終更新者: nishio

## テロメアのサマリー

- nishio	更新期間 2022/5/26 〜 2022/5/26	19行更新

## 本文

Scrapbox Integration
[https://gyazo.com/161582ef0cc55da4a64a3e8b5f18d64f]

This is an experimental feature.
	Scrapbox integration is turned on by specifying the Scrapbox project name in the Ba details dialog:
		Kozane text is parsed as Scrapbox notation
		Icon notations will be rendered as images
		Link notation will be rendered as blue text

I avoid making links as they are because I think that if I make them as they are, there will be accidents when people try to drag the kozane and open the link.
	The reason is that "moving the kozane" is the most frequent action, and there is a big difference in cognitive load between "it is OK to mouse down anywhere in the square" and "you have to look carefully and mouse down where there is no link".
	I once tried to make the setting menu appear by putting a hit detection on arrow annotations, but this also interfered with "moving the kozane", so I stopped!

How to use.
	[https://scrapbox.io/files/623d1d7e496ca3001e2b1920.png]
	[https://scrapbox.io/files/623d1d969da5e8001f3b080c.png]

	The implementation switches the rendering of kozane, so you can switch even with existing Ba


-------------------- Related Pages --------------------

## 1 hop link

- Release Notes
