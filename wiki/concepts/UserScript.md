---
title: Kozaneba UserScript
type: concept
created: 2026-05-16
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2022-05-26__KozanebaのUserScript.md
  - wiki/themes/CI安定化とVite移行_2026-06.md
---

## 定義

[Kozaneba](../entities/Kozaneba.md) のユーザがブラウザ上で実行できる **JavaScript による機能拡張機構**。`kozaneba` グローバルオブジェクトを通じてイベントフック、メニュー追加、定数変更、関数呼び出しができる。

## サンプル機能

[KozanebaのUserScript](../../raw/scrapbox_kozaneba/2022-05-26__KozanebaのUserScript.md) より:

- `kozaneba.after_render_toppage` — トップページレンダリング後のフック
- `kozaneba.user_buttons.push({label, onClick})` — ボタン追加
- `kozaneba.show_dialog("AddKozane" | "User")` — ダイアログ表示
- `kozaneba.constants.group_padding = 5` — 定数変更
- `kozaneba.user_menus.Scrapbox.push(...)` — Scrapbox こざね用メニュー追加
- `kozaneba.user_menus.Selection.push(...)` — 選択範囲メニュー追加
- `kozaneba.user_menus.Gyazo.push(...)` — Gyazo こざね用メニュー追加
- `kozaneba.user_menus.Main.push(...)` — メインメニュー追加
- `kozaneba.get_clicked_item()` / `kozaneba.get_selected_ids()` / `kozaneba.get_global()`
- `kozaneba.fit_to_contents(ids)` / `kozaneba.reset_selection()`
- `kozaneba.add_kozane(text)`
- `kozaneba.update_style(target, fn)` / `kozaneba.redraw()`
- `kozaneba.toggle_physics()`
- `kozaneba.unpin(ids)`
- `kozaneba.constants.to_make_local_backup = true`
- `kozaneba.constants.add_kozane_dialog_is_fullscreen = true`
- `kozaneba.constants.fontsize_of_add_kozane_dialog = "24px"`

## セキュリティ

[Kozanebaのコードを丸ごとo1 Proに入れる](../../raw/scrapbox_kozaneba/2024-12-14__Kozanebaのコードを丸ごとo1_Proに入れる.md):

> UserScriptDialog 内で eval を用いてスクリプトを実行しています。これはセキュリティリスクになる可能性があります。ユーザー入力を eval せず、sandbox 環境や限定的な API 経由で実行するなどの対策が望まれます。

## Vite の direct eval warning

[CI安定化とVite移行 2026-06](../themes/CI安定化とVite移行_2026-06.md) で、Vite build が direct `eval` warning を見えるようにした。

現時点では、これは UserScript の仕様変更として扱わない。UserScript は Kozaneba の利用者が意図的にコードを実行し、`kozaneba` API を通じて自分の作業環境を拡張するための機構である。したがって、sandbox 化や実行文脈の変更は別の製品判断であり、warning 消しの小修正に混ぜるべきではない。

次に扱うなら、既存仕様を維持したまま、Vite / minifier warning、production console noise、Sentry に送るべきでないユーザー試行錯誤ログを分ける小さな作業にする。#4 production console log cleanup の後で、direct `eval` が intentional であることをコード上で明示する、または挙動を変えない形で warning の扱いを整理する。

## 関連

- [コンヴィヴィアリティ](コンヴィヴィアリティ.md) — UserScript はユーザが「形を与える自由」を持つコンヴィヴィアルな設計の一例(明示的な紐付けはソースになし、推測)

## Sources

- [KozanebaのUserScript](../../raw/scrapbox_kozaneba/2022-05-26__KozanebaのUserScript.md)
- [CI安定化とVite移行 2026-06](../themes/CI安定化とVite移行_2026-06.md)
