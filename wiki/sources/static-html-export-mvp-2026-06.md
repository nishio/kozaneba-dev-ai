---
title: 静的 HTML export MVP 2026-06
type: source
created: 2026-06-03
updated: 2026-06-03
sources:
  - work/kozaneba-static-html-export/src/StaticExport/build_static_html.ts
  - work/kozaneba-static-html-export/src/StaticExport/download_static_html.ts
  - work/kozaneba-static-html-export/src/StaticExport/build_static_html.test.ts
  - work/kozaneba-static-html-export/src/AppBar/MainMenu/MainMenu.tsx
  - https://github.com/nishio/kozaneba/pull/47
---

# 静的 HTML export MVP 2026-06

## 位置づけ

「過去の作品が壊れずに見られる状態をキープしつつ、新しい外見を試行錯誤したい」という要求から出た実装メモ。

別 deploy に古い renderer を残す案は現実的だが、それより強い保存手段として「その時点の Ba data と read-only viewer を 1 つの HTML として download する」MVP を実装した。これは [Kozaneba コード構造調査](kozaneba-code-architecture.md) の「場 = Firestore doc 相当 JSON」という現行構造に乗っている。

PR は [nishio/kozaneba#47](https://github.com/nishio/kozaneba/pull/47)。branch は `codex/static-html-export`、commit は `1e3d09e Add static HTML export`。

## MVP の範囲

今回やる範囲:

- Main menu に `Download Static HTML` を追加。
- 現在の Ba を `state_to_docdate(getGlobal())` で Firestore doc 相当 JSON に変換。
- HTML 内の `<script type="application/json" id="kozaneba-data">` に Ba JSON を埋め込む。
- HTML 内に軽量 read-only viewer を同梱し、ブラウザで単体表示できるようにする。
- こざね、open / closed group、Scrapbox card、Gyazo card、line label、pan / zoom / fit を表示する。
- 外部画像は URL のまま残す。

今回やらない範囲:

- Gyazo / Scrapbox / favicon 画像の data URI 化。
- `index.html + assets/` の zip export。
- 現行 React bundle そのものを inline 化する完全再現 viewer。
- 静的 HTML 上での編集・保存。
- schema migration 付きの長期 archival format 設計。

## 実装構成

`work/kozaneba` が dirty だったため、clean worktree `work/kozaneba-static-html-export` を detached HEAD から作り、そこで実装した。

追加ファイル:

- `src/StaticExport/build_static_html.ts`
  - Ba doc と title から HTML 文字列を生成。
  - `</script>` などを含むこざね text で HTML が壊れないように JSON を escape。
  - viewer script / CSS を HTML 内に inline。
  - file name 用の `make_static_html_filename` も提供。
- `src/StaticExport/download_static_html.ts`
  - `state_to_docdate(getGlobal())` で現 state を doc 化。
  - `build_static_html` で HTML を生成。
  - `Blob` と temporary `<a download>` で `.html` として download。
- `src/StaticExport/build_static_html.test.ts`
  - JSON 埋め込み escape と file name sanitize の unit test。
- `src/AppBar/MainMenu/MainMenu.tsx`
  - `Download Static HTML` menu item を追加。

## なぜ軽量 viewer なのか

現行 React app bundle をそのまま静的 HTML に押し込むと、Firebase / Sentry / Google Analytics / UserScript / auto-save などの production app 由来の副作用も持ち込むことになる。過去作品保存の MVP では、編集や同期より「将来ブラウザで読めること」が重要なので、軽量 read-only viewer にした。

この viewer は現行 UI の完全再現ではない。表示対象は読み取りに必要な構造へ絞る:

- Kozane: text / position / scale / custom style / URL link mark
- Group: open / closed 表示、group title、children
- Scrapbox: title / image or description
- Gyazo: thumbnail URL
- Annotation: line / arrow head / label
- View: fit / pan / zoom

## 外部画像の扱い

今回の結論は「外部画像は URL のまま」でよい、という MVP 判断。

ブラウザの「Webpage, Complete」で保存してもらう案は補助的には使えるが、保存対象、lazy load、redirect、認証、URL 書き換え、browser 差分に依存する。長期保存の仕様としては弱い。

完全アーカイブが必要になったら、次のいずれかへ進む:

- app 側で画像 URL を列挙して fetch し、data URI として HTML に埋め込む。
- HTML と assets directory を zip で出力する。

ただし、この改善保存は今回の scope から外した。

## 検証

local 検証:

- `npm test -- src/StaticExport/build_static_html.test.ts`: pass
- `npm test`: pass
- `npm run build`: pass
- `npm run codex:preflight`: pass

in-app browser では `http://127.0.0.1:3011/#blank` を開き、Main menu に `Download Static HTML` が表示されることを確認した。Codex in-app browser は download event 非対応のため、actual download event はそこで検証できなかった。

PR 作成直後、GitHub Actions は running だった。

## 今後の判断点

- PR #47 を merge した後、実 browser で download file を開く smoke test を 1 回行う。
- 旧 renderer 保持と静的 HTML export は競合しない。前者は online viewer の互換性、後者は作品ごとの snapshot 保存。
- schema を壊す新 item type を追加する場合、static viewer 側にも fallback 表示を足す必要がある。
- 長期 archival format にするなら、viewer version と doc schema version の互換表を別途持つ。

## Sources

- [build_static_html.ts](../../work/kozaneba-static-html-export/src/StaticExport/build_static_html.ts)
- [download_static_html.ts](../../work/kozaneba-static-html-export/src/StaticExport/download_static_html.ts)
- [build_static_html.test.ts](../../work/kozaneba-static-html-export/src/StaticExport/build_static_html.test.ts)
- [MainMenu.tsx](../../work/kozaneba-static-html-export/src/AppBar/MainMenu/MainMenu.tsx)
- [PR #47: Add static HTML export](https://github.com/nishio/kozaneba/pull/47)
