---
source: https://scrapbox.io/kozaneba-forum/Release_Notes
fetched_via: cosense CLI v1.4.4 (browsePage)
fetched_at: 2026-06-03
page_created: 2021-08-19
page_updated: 2025-09-11
lines: 449
chars: 22916
author: nishio
---

# Release Notes (kozaneba-forum)

Kozaneba 公式 forum (https://scrapbox.io/kozaneba-forum/) の Release Notes ページのスナップショット。
2021-06-25 (Movidea→Kozaneba 改名前の最初の検証) から 2025-09-11 (Merge 挙動変更) までの全リリースノートを時系列降順で含む。

注: kozaneba-forum-ja は存在しない (404)。日本語版 forum は未作成。

---

Release Notes
2025-09-11
	Merge Behavoir Changed
		Before: Merge always created a new kozane and deleted all selected kozanes
		After: The kozane with highest scale value survives, others are deleted
		Edge case: When multiple kozanes have the same maximum scale, text combination logic applies only to those large kozanes, and the first one becomes the survivor
		https://github.com/nishio/kozaneba/pull/33
		new behavoir
			[https://gyazo.com/f4fea44776b9e5848bf2384379de4dc7]
			[https://gyazo.com/33e95419f0a3af5b67ecc170813d45f1]
		old behavior
			[https://gyazo.com/ded5d51cbee922f67ba98071571fa28d]

	Add Scale Double command to Group menu
		Adds a new "Scale Double" command to the Group context menu that doubles both the spacing (position) and size (scale) of items within a group. This complements the existing "Spread" command which only doubles the spacing between items.
		[https://gyazo.com/adcf718feb2429a8babed153f70439e7]
		https://github.com/nishio/kozaneba/pull/34
	Rotate, Spread and Scale Double are on Selection Menu too
		https://github.com/nishio/kozaneba/pull/35

2023-02-28
	Rotate
		[https://gyazo.com/9a87d3147367678c05a557abd10a9770]
		Rotate the group counterclockwise
		After taking this screenshot, I added an icon to make it easier to see the direction of rotation
			[https://scrapbox.io/files/63fd9655afcfb2001cc7bf45.png]
	Spread
		[https://gyazo.com/b5b6fbaefcf04c8b23c31da960b00515]
		Stretches the whole group by a factor of 2
		This function is used when the items are too dense and it is difficult to add items in between

[2023-02-27]
	[Tear / Merge] added.
		[https://gyazo.com/96efdbee11b617cbc8e99081a782c4d2]
		Tear
			Tear tears apart a kozane that has two or more lines connected to it.
		Merge
			Merges a selection of kozanes into one.
	[Remove Split-Kozane feature]

2023-02-06
	Dragging a target is also considered selection of the target to draw a line.
		[https://gyazo.com/fc0bb02b2af5bf358decc3d911228707]
		When selecting an object in line-drawing mode, if the pointer moved, the "line drawing by clicking" was not triggered because it was a drag.
		This behavior was in accordance with the specification, but from the user's point of view, it appeared that the line was not drawn for some reason.
		So, I changed it so that dragging an object is considered a selection.
	The menu item "Draw lines" has been partially changed.
		[https://gyazo.com/5aaad4337df6384b0717cb8c0c1cfc6e]
		In some cases, especially when a tree-like structure is used, you may want to draw a line from a parent element to multiple child elements, but until now there was only a menu to draw arrows from parent to child.
	Change in specification for drawing a line from one element to another
		Previously, the center of gravity was used to separate the left and right sides of an element, rather than the midpoint, which was sometimes counter-intuitive when there were many elements.
		From now the midpoint is used as the border.
		before / after
		[https://gyazo.com/3b6bcca805f4d6f85ffe9a3d5ca83221] / [https://gyazo.com/2e9b3c847e118e0bc627eb38decbe9ef]


2023-01-26
	A menu to make a line has been added to the context menu when clicking on an element such as a kozane or a group.
		After selecting the menu, clicking on another element will make a line
		Clicking on an empty space cancels to make a line.
		[https://gyazo.com/2f0c666d1a8f3bbee59f0868ccf524c0]
		You can also delete a line in the same way.
			[https://gyazo.com/f31c63af7bb9bba26c8db29d95493b0d]
			You can delete a line between two selected elements.
			The direction of the arrow does not matter.
				It does not matter whether you use the root or the tip context menu
	Enter key opens AddKozane Dialog
	Added `kozaneba.constants.to_adjust_opacity_of_lines_by_length` to the API
		Kozaneba sets higher transparency for longer lines.
			This is an expression of "related but the relation is not important" because they are placed far apart.
			Otherwise, only the long lines connecting distant objects will stand out, and the short lines will be unnoticeable.
		I think this behavior is correct as the default for a stationery tool to organize thoughts.
			However there is a use case where Kozaneba is used to create a diagram to communicate something to others, which is also rather useful.
			In such cases, it is sometimes better not to have different line colors.
			So I made it possible to switch between them.
		`kozaneba.constants.to_adjust_opacity_of_lines_by_length` default: true
			true / false
			[https://scrapbox.io/files/63e0c69874214f001ec56000.png][https://scrapbox.io/files/63e0c69db66af2001d0c2973.png]


2023-01-16
	If you try to create a Kozane for a Scrapbox page that does not exist.
		[https://scrapbox.io/files/63c511435aba3f001d9cdc78.png]
		A page which are linked to from somewhere but have no content yet is shown as empty.
			Same behavior as before
		A page which not linked from anywhere is shown as `Page not found`.
		If there is no such project, `Project not found` is displayed.

2022-07-06
	Enlarged group title font-size
		new
			[https://scrapbox.io/files/62c501565a015a001d5e5bd3.png]
		old
			[https://scrapbox.io/files/62c50166be91de001d1a5ae9.png]
	Add extra menus for resizing objects (kozane, closed groups, images, Scrapbox kozane).
		Up to now, only big/small was available to resize +1/-1.
		However, this was inconvenient when you wanted to increase the size of objects that was already large.
			For example, if you wanted to increase the scale from 4 to 8, you had to click the menu four times.
		So I added a menu to double/half the size.
			[https://gyazo.com/194b7c20dbfc569b21c7f5b9adefbc94]

2022-07-05
	I noticed that if the number of objects present in Ba exceeds 2048, it will not be possible to save to the cloud.
		Adding more than 2000 objects in one Ba is not useful to understand the whole picture, so we should split the Ba.
		(I was doing it as a rendering load test and experimenting to see how far I could go, and this happened to me for the first time. It is not a normal usecase.)
	I added a information message when the number of objects exceeds 1000.
		And when the number of objects exceeds 2000, the warning message will continue to be displayed.
	[https://scrapbox.io/files/62c3fd68dbb0ce001d67fcfa.png]
	[https://scrapbox.io/files/62c3fd843562ae002300dd86.png]


2022-06-16
	Kozaneba's Ba can be converted to PDF by "Print to PDF".
		This PDF can be enlarged to read even the smallest text, so if you open all groups and make PDF, you can read every detail of the Ba even without internet access.
	However, at least in Chrome, there is a phenomenon that the height of lines is wrong only when printing on some Kozane.
		Left: on-screen display / Right: when printed to PDF in Chrome
		[https://scrapbox.io/files/62aaeb1c043b87001d71e676.png][https://scrapbox.io/files/62aaeafe9d466f001d3a98e6.png]
	The root cause of this phenomenon and the best workaround are still unknown, but a smaller font size is better than losing information, so I reduced the font size to 80% only when printing.
		[https://scrapbox.io/files/62aaeb1c043b87001d71e676.png][https://scrapbox.io/files/62aaeb4d8132eb001d6ff917.png]
	I welcome any information on this issue.

2022-06-09
	When [Scrapbox Integration] is turned on, a menu "expand scrapbox links" appear on the context menu of Kozane and Group.
	It interprets Scrapbox link notation and imports all link targets together.
	[https://gyazo.com/0dbed547242f0badd75adb4e5fd93c69]

2022-06-08
	Due to a modification made on 5/26, there was a bug that caused the arrows to be incorrectly positioned when the object with arrows are in a group. Fixed.

2022-06-03
	More image URL patterns supported:
		When pasting a URL,
			previously we only support Gyazo URLs `https://gyazo.com/~`
			now we consider URLs ending with `.png` as image URL
		In the Add Kozane dialog,
			previously each line is considered as text
			now some pattern are considered as image URL
				Gyazo URLs `https://gyazo.com/~`
				`http~.png`
				The above enclosed in `[]`
			If a line contains multiple `[]`, each is added as a separate image.
			With this modification, you can select and copy from the image-mixed Scrapbox, and directly paste them into Kozaneba
	Fixed a problem in which dragging did not work as intended when image kozanes overlap.

2022-05-30
	Kozane with external link now have favicon of the target domain.
	[https://gyazo.com/a2c4c79ae9cae536ae7d5325a3824ac1]

2022-05-26
	A question mark button has been placed in the upper right corner for help. This is the same behavior as the one in the status bar, but to make it easier to find it.
	Improved display of arrows:
		The arrow heads and the body of the arrows are now translucent together.
		Opacity now changes smoothly depending on the length of the arrow
		Arrows with branches and double lines that were previously not translucent are now translucent like single arrows.
		[https://scrapbox.io/files/628f38a65cd850001d0dc32f.png]
		Previously, heads of arrows were 100% opaque black.
			[https://gyazo.com/b409d614e2e158f361ba980172136b11]

2022-03-24
	[Scrapbox Integration]
	If you specify a scrapbox project name, the text of kozane are parsed as scrapbox format. Icons are rendered as images.
		[https://scrapbox.io/files/623d1e53b23db5001d6e37cb.png]

2022-01-25
	Dragging the Middle Button to Move the Viewpoint
	"Leave from lines" menu
		Leave from lines and arrow associations
		[https://gyazo.com/f83f83f555be85ab03d5b9d457b9ec98]

2021-12-27
	Bug fix:
		When two or more groups are nested, the endpoint of the arrow for the group was wrong.
		When dragged an item from a nested group, the item did not move to the position it was dropped.

2021-12-22
	Fixed a bug where dragging on a group outside of the selection would hide the group.
	Dragging outside the selection now simply deselects the selection.
	Selection is now automatically deselected when there is nothing to select.

2021-10-12
	[https://gyazo.com/8856cb7b7951c957dfdfe3cf6f27f15a]
		Added the doubled line/arrow and the line without arrow heads
		In previous version, the statement "this and that are the same" is only expressed by proximity. From current version, you can simply express it with doubled line.
			[https://gyazo.com/3ab0ab2660f69574e9bccef3e5481b2d]
	In previous version, when close a group with some large items in it,  the closed group was default small size. Now the group become the max size of its children.

2021-09-28
	The "add arrow" is now images, making it easier to see what will happen.
		[https://gyazo.com/5b240c04a1f83c891eba3a279609964b]
		Added the ability to draw arrows in batches with a user script" on 2021-09-16.
		On 2021-09-17, "Add double-headed arrows to the menu since they are used frequently" and "Also, specify that the right side is the head in the menu for adding arrows".
		These three changes were made to the menu in the image.

2021-09-17
	Added double-headed arrows to the menu as I use them frequently.
		The previous menu to add single-headed arrows also clearly states that its head is right.
	Fixed an error when an element is removed from an untitled group with an arrow and the group is automatically deleted.

2021-09-16
	Reduce physics vibration.
	Moved `unpin` to the User custom menu instead of the System menu.
		Since the physics engine itself is still an experimental function that can be added and used by user scripts, it is misleading to display it even to those who have not added it.
	Create an API for adding arrows.
		Added the ability to draw arrows in bulk by user script.
		[https://gyazo.com/db24cc8fbda75c98c436458fb4fcf5fc]
	Adjusted the position and color of link icons
	Added a server API to create a Ba with JSON.

2021-09-09
	Added UserScript dialog to check the current code, rewrite it, run it, and save it.
		Until now, the only way to use UserScript was to rewrite localStorage directly.
	Arrows can now be added from the selection menu.
		[https://gyazo.com/cef794096b06743c1bbd9b9c4b40f0c4]

2021-09-08
	An `Annotation Layer` has been added.
		Currently, only arrows are implemented.
		GUI has not been created yet, can only be created by importing JSON
	Automatic shaping by physics engine added.
		Can be turned on/off only via API

2021-09-03
	When turning on cloud saving, only the user who did it have the right to edit.
		When user Y opened a Ba created and shared by user X, and saved it as an alias, the information that X had editing privileges was saved.
		Because of this, the Ba saved by user Y was displayed in the list of user X's lists and could be rewritten.
	Automatic backup to the browser's IndexedDB
		Currently default: off, will turn default: on after using it myself for a while
	Added the ability to save backup data to a new Ba

2021-09-02
	When ungrouping a group with a title, an empty group with that title remains in the upper left corner
		Previously, the title was in the new Kozaune.
			The idea is that the title of a group is a new summary of the group, and should not be deleted.
		By keeping the title as a group instead of Kozane, for example, if you want to put half of a group with many elements into another group, you can ungroup it, select half and put it into the original group, and put the other half into another group.
		[https://gyazo.com/c1dca0c13c52d22defed38e53e39921b]
	Fixed a bug in pasting
	Delete Ba
	Link Kozane
		if the `custom.url` of the Kozane is neither empty string nor undefined:
			a link icon will appear in the upper right corner and `visit` will be added to the menu
		When you paste a URL, a kozane with `custom.url` will be created.
			The string to be displayed can be edited from the context menu after pasting.
	Fixed a bug in Firestore permission rules.
		It was possible to delete a read-only shared Ba by running the development server locally and executing the delete command from the development menu.
	Fixed a problem where there was no place to grab a group to drag it if it was lined up exactly in a rectangle when adding some Kozane.
		Fixed padding when group has low space ratio.
	Automatic deletion of empty groups with no title.

2021-09-01
	Move groups to the front when opening/closing groups
	Move group to Front on API Styling
	Fixed a bug in which the position of a group is shifted when it becomes empty.
	Fixed a bug that caused a discrepancy between the on-screen position and the internal position when moving a group in and out.
		It will be a bit more expensive to update, but hopefully the strange misalignment will no longer occur.
	Fixed a bug that caused a crash when MouseUp on a group while selecting a range.
	Selection can be copied (copy JSON)
	It can be pasted.
		This JSON pasting is exposed as an API, so users can create a template-like object from the custom menu.
	The last saved time can be checked from the StatusBar.
	Fixed a bug where deleting a group and a tickle did not trigger a save
	Zoom out with N key
	Fixed a bug that caused a crash when ungrouping a group within a group
	Added sample user scripts
		Menu to maximize selection
		Emoji preset kozaune creation menu
		Copy Scrapbox Cozine Title menu
		Menu to Make White Background Transparent for Image Cozines

2021-08-31
	When pasting in a browser that can't read from the clipboard (Firefox, etc.), a prompt appears instead.
	Can be added to the menu by user script.
	Resolved excessive screen redraw.
		Performance is better, but I'm sure there are a few places where I'll forget to refresh the screen.
	Bug in group not being updated when closed
		Cut back to yesterday's step.

2021-08-30
	Scrapbox Kozane and Gyazo Kozane have been added.
	Pasting the URLs of those services will create a kozane.
		For Scrapbox, you can only add individual pages, not the project top page.
	You can visit the original service by clicking the kozane.
		[https://scrapbox.io/files/612c83f9c6c197001d487010.png]
	Fixed a bug that causes a crash when the Scrapbox project top page is posted.
	Fixed a loading dialog when a Scrapbox link is posted because it was confusing to see nothing while waiting for it to load
	Fixed a bug that images are not displayed when Scrapbox is set to upload images to Scrapbox instead of Gyazo.
	Added edit and clone functions to KozaneMenu
		It used to be possible to clone by adding without editing with split, and edit by editing and replacing.
		However, the frequency of edit is overwhelmingly high in actual use. But when I used it, I found that the frequency of edit was overwhelmingly high, and it was inconvenient that there was no shortcut key for replace.
		So I separated them. If you don't feel the need to keep split after using it for a while, I plan to remove it.
	Added favicon
	When you press the expand menu of Scrapbox Kozane, all pages within 2hop will become Kozane.

2021-08-28
	Can make copy of the Ba

2021-08-27
	The `title change` menu in the main menu disappeared and was replaced by [Ba Dialog].
	The following can be done from `Ba Dialog`
		Change the title
		Change whether or not to give editing privileges to people who know the link.
			If you don't give edit permission, you can do read-only sharing.
			Example of read-only sharing: https://kozaneba.netlify.app/#view=23YyYfzS1dTx05tugVW7

2021-08-26
	[User Dialog]
		The list of Ba is sorted by update time.
		You can create a new Ba.
			Same if you access `/#new` while logged in
	Group padding can now be adjusted. see [Change constants].
		Considering whether to make the default value smaller.
	Ability to add custom styling. see [User scripts].
	Make sure groups don't get smaller than a certain size even if they are empty.

2021-08-25
	Include untitled group contents in the default value when editing group titles.
	Fixed a bug in which creating a group and deleting a selection were not considered as updates and were not saved in the cloud.
	Fixed a bug in which the context menu was temporarily shifted when clicked.
	Improved loading display.

2021-08-23
	Set zoom speed for wheel and touchpad separately. see [Modify Constants].
	[User Scripts].
		Allow users to define scripts to be executed at load time.
		User can override the behavior of the tutorial when accessing the top page.
		User can add a button to the AppBar.
	Dragging a closed group will cause it to shift position.
		Bug in which the display position is misaligned when a closed group is placed in a group.
		Bug in which the position of the upper left corner of the group is changed by dragging inside the group.

2021-08-20
	Beta release
	Fixed a bug that hotkeys are triggered even in the feedback dialog.
		→Fixed a problem where hotkeys did not work as a side effect of this fix.
	Some constants can now be modified by the user: [Modify Constants].
	The AddKozane dialog is no longer closed by pressing ESC or clicking on the background.


2021-08-19
	User dialog
		when you select User from the main menu
		You will see a list of places you have edited in the past.
	When you finish the tutorial, you can open the help from the Hathena icon.
	You can now select a range and Copy Text.
	Fixed a bug in the movement speed of the selected range.
	Zoomed-in items can be reduced.
	Zoom in with b

2021-08-18
	The scroll bar in the tutorial is not visible in Chrome on Mac, so you can't notice that there is more content.
	Added Replace button to Split dialog.
		Replaced the selected Kozane.
	When opening an existing Ba, the entire content will be visible in full view
		To solve the problem of not being able to see what's off-screen, or not being able to see what's on the initial screen by accident. If you don't see something on the initial screen, you might be in a hurry.
	Use the spacebar to toggle between full view and full view.
	AddKozane, SplitKozane, and EditGroupTitle dialogs are now equivalent to Cmd+Enter to click the decision button.
	Fixed a problem that the border of a kozane was covered by the neighboring kozane by 1 pixel when adding a kozane.

2021-08-17
	Scaling is now centered on the mouse cursor position.
	Closed group nameplate display no longer has yellow background.
		Fixed the problem of not being able to distinguish between a closed group and a group without a title that contains only one kozane.
	Stopped using the browser's DragDrop API.
		The display of dragged items is not always the same depending on the environment.
		After using it for a while, I decided that it would interfere with placing dragged items in the desired position.

2021-08-13
	Can open and close groups from context menu
		Ability to open and close groups from the context menu.
		Ability to edit the title of a group.
			The title is an empty string by default, and is not displayed when open, but the combined contents are displayed as the nameplate when closed.
		Ability to split a Kozaune.
	Bug fix for dragging and positioning of closed Kozaune.
	Fixed a bug where clicking on a Kozaune in a group would bring up the group menu.
	Fixed a bug when deleting a Kozaune in a group.

2021-08-12
	Fixed a bug in the sign-in dialog that an error occurred and you could not sign in.

2021-08-11
	Fixed a bug in which the sign-in dialog did not appear when trying to start a cloud save.
	2021-08-11 Fixed a bug that the sign-in dialog did not work when trying to start saving to the cloud.
	Fixed a bug where the feedback function would not work.
	The vertical wheel will move up and down (= vertical scrolling). The user was expecting the shift key to scroll horizontally, so the X and Y of the wheel event are exchanged when the shift key is pressed.
		Since [nishio.icon] doesn't usually use the wheel, feedback from those who do would be welcome.

2021-08-10
	Introduced Sentry to send detailed information about errors that occur in the hands of users to the developers.
	Hide the developer menu from all users except localhost.
		It only appears if the login user is a specific user.
	Added tutorial pages (~16)

2021-08-06
	When opening from a phone and the screen size is small, a message "It's small! message is added to the tutorial when the screen size is small.

2021-08-05
	Added a tutorial function.
	Added the ability to save to the cloud.

2021-08-03
	Added Google integration to sign in.

2021-08-02
	First release
		We used to call it Movable Ideas (Movidea), but changed to Kozaneba on this occasion.
	Anonymous User Account Creation

2021-07-28
	Additional features of Kozaneba
	Release and delete groups
	Delete a Kozane
	Group selected items, delete them all at once

2021-07-27
	In and out of groups
	Make group background semi-transparent
	Deselect When Dropping into a Group

2021-07-26
	Added Routing
	Allow dragging of Kozane in a group.

2021-07-20
	Move Selected Items by Dragging Selection

2021-07-19
	Range Selection

2021-07-15
	Dialog Display Function
	Changed Piece to Kozane and Canvas to Ba.
	Up bar became translucent
	Menu and context menu functions
	Move Groups by Dragging

2021-07-14
	Imported JSON made by range selection copy from Regroup, and reproduced with almost the same layout

2021-07-13
	Title Display Function for Groups

2021-07-02
	Combining Cypress and ReactN to be able to test
	Parallelism Scaling with Touchpad
	Drawing a Group (Basic)
	Importing from JSON in Regroup (Layout will be broken)

2021-06-25
	Verify if it is possible to automatically adjust font size without using Canvas -> Done.
