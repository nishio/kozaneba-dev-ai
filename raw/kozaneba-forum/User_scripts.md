# User scripts

## メタデータ

- タイトル: User scripts
- 作成日時: 2021-08-28T15:52+09:00 (5 years ago)
- 最終更新日時: 2021-08-28T15:54+09:00 (5 years ago)
- 最終アクセス日時: 2026-05-23T01:24+09:00 (2 weeks ago)
- 被リンク数: 2
- pageRank: 2.2
- views: 43
- 行数: 25
- 文字数: 861
- 作成者: nishio
- 最終更新者: nishio

## テロメアのサマリー

- nishio	更新期間 2021/8/28 〜 2021/8/28	25行更新

## 本文

User scripts
Allows users to define scripts to be executed at load time.
code:js
	localStorage.setItem("onLoad", "console.log('hello')")

Users can override the behavior of the tutorial when accessing the top page.
`kozaneba.after_render_toppage = () => { kozaneba.show_dialog("User") }`

User can add a button to the AppBar
code::
	localStorage.setItem("onLoad", `
	kozaneba.after_render_toppage = () => { kozaneba.show_dialog("User") }
	kozaneba.user_buttons.push({ label: "Add", onClick: () => { kozaneba.show_dialog("AddKozane") }
	`)
[https://scrapbox.io/files/61237d14cc0eb2001df080d5.png]
Ability to add custom styles.
	[https://gyazo.com/b202a0b941bbc79c1a9ef348b654a89e]
	`kozaneba.update_style("1629979178768", (s) => {s.background = "blue"; s.color = "white" });`

API
	https://github.com/nishio/kozaneba/blob/main/src/API/KozanebaAPI.ts


see [/kozaneba-forum-jp/ユーザスクリプト]


-------------------- Related Pages --------------------

## 1 hop link

- Release Notes
- Modify Constants
