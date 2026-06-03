# FAQ

## メタデータ

- タイトル: FAQ
- 作成日時: 2021-09-28T17:43+09:00 (5 years ago)
- 最終更新日時: 2021-09-28T17:45+09:00 (5 years ago)
- 最終アクセス日時: 2025-09-23T22:03+09:00 (8 months ago)
- 被リンク数: 0
- pageRank: 0
- views: 12
- 行数: 12
- 文字数: 763
- 作成者: nishio
- 最終更新者: nishio

## テロメアのサマリー

- nishio	更新期間 2021/9/28 〜 2021/9/28	12行更新

## 本文

FAQ
	Q: Zooming in and out with the wheel is too fast.
	A: 
		The wheel and touchpad have different sensitivities, and Kozaneba is set for touchpad by default.
		It would be best if it could detect that the user is using the wheel and switch between the two, but we don't know how to do that.
		This identification function is customizable with the name `kozaneba.is_touchpad`, so you can experiment with it if you are interested.
		If you are not interested in the identification method and you just use the wheel, so you can use `kozaneba.is_touchpad = () => false;`".
		UserScript Dialog:
			[https://gyazo.com/f6f57a933c79715b1b2a9e0fb5d2f3d1]
			You can open the dialog from `EDIT USERSCRIPT` in the User dialog.
			[https://gyazo.com/c4d9aa3f9f1fe65c031038cb8d3553ae]

