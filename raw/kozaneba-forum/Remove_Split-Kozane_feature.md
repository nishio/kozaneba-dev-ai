# Remove Split-Kozane feature

## メタデータ

- タイトル: Remove Split-Kozane feature
- 作成日時: 2023-02-27T17:46+09:00 (3 years ago)
- 最終更新日時: 2023-02-27T17:53+09:00 (3 years ago)
- 最終アクセス日時: 2026-06-02T20:48+09:00 (16 hours ago)
- 被リンク数: 1
- pageRank: 4
- views: 31
- 行数: 28
- 文字数: 1874
- 作成者: nishio
- 最終更新者: nishio

## テロメアのサマリー

- nishio	更新期間 2023/2/27 〜 2023/2/27	28行更新

## 本文

Remove Split-Kozane feature
[2023-02-27].
Removed Split Kozane feature.
	This was an overly abstract feature created at a time when it was not yet clear what kind of functionality was needed.
	The [Edit Kozane] and [Clone] functions are sufficient.
	Editing with the Edit Kozane feature replaces Kozane.
		[https://gyazo.com/dc88bce8e7d7026200144882cd031f52]
	If you make multiple lines at this time, you will have multiple copies.
		[https://gyazo.com/c4c13c5426c9f11e3ffa3b09ff1e68b9]
	So the need to "add something and later split it into multiple pieces" can be realized with the Edit Kozane function.
	The Split Kozane dialog had two buttons, "replace" and "add"
		The idea was "Sometimes we want to keep the existing one and add a split of it."
		Now that the [Clone] function has been added, you can clone item when you think "I want to keep this one".
		[https://gyazo.com/595f87314e6510fb529520abd2b0e69c]

Notes on the current specification
	When you edit a kozane, if the text is a single line, it is interpreted as editing the displayed text.
		Therefore, the drawn line is maintained.
	If the text is multiple lines, it is interpreted as splitting the kozane.
		In this case, "old kozane" disappears and "import of multi-line text" is performed.
		Therefore, lines drawn on the old sticky disappear.
	The clone is interpreted as "adding a new item with the same contents," so lines are not drawn.
	[https://gyazo.com/97dfa6c1c9e8e2a2b71fc23182f363e7]
	I thought it might be a problem when I was writing the explanation, so I clearly stated it.
		I have not had any trouble with this specification so far.
		If you have a choice between a default specification with more lines and a default specification with no lines, you can say that you choose the one with no lines.
		I think it is better not to increase the number of lines because people get confused when there are too many lines.


-------------------- Related Pages --------------------

## 1 hop link

- Release Notes
