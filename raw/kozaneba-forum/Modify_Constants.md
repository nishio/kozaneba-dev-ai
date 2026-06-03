# Modify Constants

## メタデータ

- タイトル: Modify Constants
- 作成日時: 2021-08-28T15:52+09:00 (5 years ago)
- 最終更新日時: 2021-08-28T15:59+09:00 (5 years ago)
- 最終アクセス日時: 2026-04-28T03:52+09:00 (last month)
- 被リンク数: 1
- pageRank: 2
- views: 73
- 行数: 27
- 文字数: 1039
- 作成者: nishio
- 最終更新者: nishio

## テロメアのサマリー

- nishio	更新期間 2021/8/28 〜 2021/8/28	27行更新

## 本文

Modify Constants
Users can adjust the behavior of Kozaneba by rewriting constants

2021-08-20
If you find the wheel scaling too much, you can reduce it.
[https://scrapbox.io/files/611fc93c5b2db30024adff73.png]
If you want to reverse the movement of the cursor, you can do so by setting it to -1.

2021-08-23
It is now possible to set the speed of zooming with the wheel and touchpad separately
code:ts
	export const constants = {
			...
			wheel_scale_speed: 10,
			touchpad_scale_speed: 100,
	};
But there is no royal way to distinguish between wheel and touchpad
	https://stackoverflow.com/questions/10744645/detect-touchpad-vs-mouse-in-javascript/62415754#62415754
	I've exposed `kozaneba.is_touchpad`, so if you want to distinguish between wheel and touchpad, please rewrite it in [User scripts]. The default function is to always identify it as a touchpad.

2021-08-26
	Make group padding adjustable.
`kozaneba.constants.group_padding = 5; kozaneba.redraw();`

Translated with www.DeepL.com/Translator (free version)
see original in [/kozaneba-forum-jp/定数の変更]


-------------------- Related Pages --------------------

## 1 hop link

- Release Notes
- User scripts
