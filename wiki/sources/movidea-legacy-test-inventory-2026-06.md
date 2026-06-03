---
title: Movidea legacy test inventory 2026-06
type: source
created: 2026-06-03
updated: 2026-06-03
sources:
  - work/kozaneba/cypress/e2e/movidea/
  - work/kozaneba/cypress/e2e/kozaneba/
  - work/kozaneba/cypress/support/e2e.ts
  - work/kozaneba/scripts/cypress-emulator-smoke.sh
  - work/kozaneba/src/Global/exposeGlobal.ts
  - work/kozaneba/src/Global/initializeGlobalState.ts
  - work/kozaneba/src/Canvas/ItemCanvas.tsx
  - work/kozaneba/src/Event/fast_drag_manager.ts
  - work/kozaneba/src/Event/drag_drop_item.ts
  - work/kozaneba/src/Event/drag_drop_item_into_group.ts
  - work/kozaneba/src/Event/drag_drop_selection.ts
  - work/kozaneba/src/Event/drag_drop_selection_into_group.ts
  - work/kozaneba/src/Event/drag_drop_state.test.ts
  - work/kozaneba/src/Event/finish_selecting.ts
  - work/kozaneba/src/dimension/item_layout.test.ts
  - work/kozaneba/src/dimension/world_to_screen.ts
  - work/kozaneba/src/dimension/world_to_screen.test.ts
  - work/kozaneba/src/Event/get_position_after_parent_change.test.ts
  - work/kozaneba/src/Event/get_total_offset_of_parents.test.ts
  - work/kozaneba/src/Kozane/AdjustFontSize.tsx
  - work/kozaneba/src/Kozane/AdjustFontSize.test.ts
  - work/kozaneba/src/Kozane/useAdjustFontsizeStyle.tsx
  - work/kozaneba/src/Regroup/importRegroupJSON.ts
  - work/kozaneba/src/Regroup/importRegroupJSON.test.ts
  - work/kozaneba/src/utils/JSON/
  - work/kozaneba/src/utils/piece_to_kozane.test.ts
---

# Movidea legacy test inventory 2026-06

## 結論

旧 Movidea Cypress spec は、現行 Kozaneba の required gate にそのまま入れる対象ではない。

Java 21 と Firebase emulator を用意して実測した結果、`cypress/e2e/movidea/` は 23 specs / 25 tests のうち 9 tests pass、16 tests fail だった。失敗の多くは現行実装の product bug ではなく、旧 state 形状、旧 UI 操作、pixel exact assertion、Cypress actionability と現行 drag 実装の衝突である。

ただし、座標変換、group 内外 drag、selection hit test、import/save/auth には回帰テストとして救う価値がある。扱いは次の 3 分類にする。

| 分類 | specs | 方針 |
| --- | ---: | --- |
| promote | 13 | 旧 spec は required gate にしない。挙動を unit / integration / Kozaneba Cypress に移す |
| rewrite-before-decision | 5 | 旧 helper や旧 menu 操作では判断不能。現行操作で再現してから残す範囲を決める |
| delete | 5 | 空 test、旧 API 検査、既存 Kozaneba test との重複、または旧 state 前提だけなので削除候補 |

## 実行条件

ローカルの Java 19 では `firebase-tools@15` が emulator 起動を拒否したため、Homebrew で Java 21 を入れて実行した。

```sh
JAVA_HOME=/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home \
PATH="/opt/homebrew/opt/openjdk@21/bin:$PATH" \
npm run cypress:emulator-smoke
```

smoke は pass した。

- `movidea/login.cy.ts`: pass
- `movidea/save.cy.ts`: pass
- `kozaneba/test_tutorial.cy.ts`: pass

その後、Movidea 全体を emulator 付きで実行した。

```sh
JAVA_HOME=/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home \
PATH="/opt/homebrew/opt/openjdk@21/bin:$PATH" \
npx firebase emulators:exec --only auth,firestore \
  "env -u ELECTRON_RUN_AS_NODE npx cypress run --spec 'cypress/e2e/movidea/*.cy.ts' --config baseUrl=http://localhost:3000,video=false"
```

結果は 23 specs / 25 tests、9 passing / 16 failing。Cypress summary は `15 of 23 failed (65%)`。

## 現行コードの前提

`window.movidea` は今も test API として expose されている。中身は `getGlobal`、`setGlobal`、`updateGlobal`、`importRegroupJSON`、`world_to_screen`、`screen_to_world`、`toUseEmulator`、`make_items_into_new_group`、`make_one_kozane` などで、旧名のまま現行 Kozaneba state を操作している。

一方で、現行 state の中心は `itemStore` / `drawOrder` であり、古い `kozane` 配列や `fusens` 配列は初期 state に存在しない。このため、`setGlobal({ kozane })` や `setGlobal({ fusens })` に依存する spec は、DOM に item が出ずに失敗する。

`hasPosition` helper は `getBoundingClientRect().x/y` の完全一致を要求する。1px 差や viewport / MUI / browser layout の差が product regression と混ざるため、旧 Movidea spec の pixel exact assertion はそのまま残さない方がよい。

drag 実装は `fast_drag_manager` が mouse down 時に対象要素へ `pointerEvents = "none"` を設定し、drop 後に戻す。旧 spec の direct `trigger("dragstart")` / `trigger("drop")` や covered element クリックは、現行の mouse-based drag flow と噛み合わない。

`ItemCanvas` は `drawOrder`、`selected_items`、`is_selected` を購読して render する。`updateGlobal` で `itemStore[id].scale` だけを直接 mutate しても、`drawOrder` を更新するか `redraw()` しない限り DOM が再描画されないことがある。現行の menu 経由の scale 操作は `redraw()` を呼ぶため、これは product bug というより test API / direct mutation の契約差である。

## Spec 別分類

| spec | 実測 | 現行コードから見た解釈 | 分類 | 次アクション |
| --- | --- | --- | --- | --- |
| `add_kozane_dialog.cy.ts` | 3 tests pass | dialog で複数行入力から group と最初の item text が作られることを確認している | promote | Kozaneba spec へ移す。旧 Movidea folder からは外せる |
| `adjust_font_size.cy.ts` | `.kozane` が見つからず fail | `setGlobal({ kozane })` が旧 state。現行 item が生成されていない | rewrite-before-decision | `adjustFontSize` の unit test と、`itemStore/drawOrder` を使う render test に分ける |
| `dimension.cy.ts` | pixel 期待値が `expected x:184 is 61` で fail | pure な world/screen 変換は現行 unit test がある。DOM left/top と group offset の旧期待が残っている | promote | `position_to_left_top`、group offset、viewport 変換を unit/component test へ移す |
| `drag.cy.ts` | pixel mismatch と `pointer-events: none` で fail | 旧 direct drag 手順が現行 `fast_drag_manager` と衝突している | promote | `drag_drop_item` の state invariant と、現行 `do_drag` 型の Kozaneba spec に分割する |
| `drag_closed_group.cy.ts` | `group-open-close` が見つからず fail | 旧 menu / close group 操作が stale | rewrite-before-decision | closed group への drag が現行仕様として必要か確認し、必要なら現行 UI 操作で新規 test 化 |
| `drag_group_to_group.cy.ts` | `selection-view` が 0x0 で click 不可 | selection/menu helper が旧 flow。group-to-group の挙動自体は別に検査する価値がある | rewrite-before-decision | group drop の state-level test として再構成できるか判断する |
| `drag_in_out.cy.ts` | 1px pixel mismatch で fail | 後続の `items` / `drawOrder` assertion は意味がある。pixel exact だけが脆い | promote | group 内外移動の親子関係、`drawOrder`、world 座標を unit/integration test に抜く |
| `drag_in_out_of_nested_group.cy.ts` | nested drag 後の y 座標期待が大きくずれて fail | root / nested 間の座標変換回帰として重要。PR #40 系の regression と重なる | promote | `get_position_after_parent_change.test.ts` と `kozaneba/test_drag.cy.ts` の coverage に対応づけ、残る gap だけ追加 |
| `group.cy.ts` | 0 tests | spec 本体がコメント化されている | delete | 削除候補。必要な regression は他 spec から救う |
| `group_regroup_json.cy.ts` | pass | `importRegroupJSON` は呼ぶが active assertion がほぼない | promote | `importRegroupJSON` の position normalization を unit test にする |
| `group_translated.cy.ts` | group translation の pixel 期待で fail | 親 group の `position` と子 item の相対位置を扱う回帰としては有用 | promote | DOM pixel ではなく group offset / absolute position の state test へ移す |
| `import_json.cy.ts` | pass | `piece_to_kozane` 後の import 表示が通る | promote | v3/v4/Regroup import を現行 JSON import path の state assertion として整理する |
| `login.cy.ts` | pass | emulator auth smoke として動いている | promote | `cypress:emulator-smoke` の一部として当面残す。最終的には Kozaneba 名の auth smoke に統合 |
| `many_fusen.cy.ts` | pass | 実質的な assertion がない。`fusens` 旧 state も現行では意味がない | delete | 削除候補 |
| `nested_group.cy.ts` | nested group の pixel 期待で fail | nested parent chain の座標回帰として重要だが、旧 DOM 期待は stale | promote | `get_total_offset_of_parents` / `get_position_after_parent_change` の coverage と照合する |
| `save.cy.ts` | pass | emulator 付き保存 smoke として動いている | promote | `cypress:emulator-smoke` の一部として当面残す。保存復元 regression に拡張する |
| `scaled_fusen.cy.ts` | width 期待が `390px` に対し actual `260px` で fail | `itemStore["3"].scale = 3` の direct mutation だけでは `ItemCanvas` が rerender しない | rewrite-before-decision | product の scale 操作は menu / API 経由で検査する。test API を残すなら `redraw()` 契約を明示する |
| `scaled_fusen_group.cy.ts` | pass | group 内の scaled kozane 表示は動く | promote | scale と group offset の小さな component/state test に移す |
| `selection.cy.ts` | selection drag 後の位置期待で fail | 前半の selection hit test は有用。後半の `#selection-view` direct drag は現行 flow とずれている | promote | `finish_selecting` の hit test と `drag_drop_selection` の state update に分割する |
| `three_fusens.cy.ts` | hidden MUI menu で fail | 旧 menu 操作が stale。group/ungroup の挙動は現行 Kozaneba spec と重複する可能性がある | rewrite-before-decision | `kozaneba/test_ungroup.cy.ts` と照合し、足りない selection -> make group だけ残す |
| `transform.cy.ts` | `+` text が見つからず fail | `setGlobal({ kozane })` 旧 state。world/screen 変換は現行 unit test がある | delete | pure transform coverage を根拠に旧 spec は削除候補 |
| `tutorial.cy.ts` | direct trigger が covered element / actionability で fail | 現行 `kozaneba/test_tutorial.cy.ts` は pass。旧 tutorial script を救う必要は薄い | delete | 現行 tutorial spec との対応を確認した上で削除候補 |
| `visit_reset.cy.ts` | `cy.window().its("x")` が存在せず fail | Cypress/browser の `visit()` reset 検査で、product regression ではない | delete | 削除候補 |

## #17 への落とし込み

Issue #17 は「テストカバレッジ向上」ではなく、以下の小さな regression 群として扱う。

- 座標変換: `dimension`、`drag_in_out`、`drag_in_out_of_nested_group`、`group_translated`、`nested_group` から、parent chain / world 座標 / group offset を抽出する。
- hit test: `selection` の前半を `finish_selecting`、`convert_bounding_box_screen_to_world`、pan/zoom 後の selection として抽出する。
- drag/drop state: `drag`、`drag_in_out`、`drag_group_to_group` から、`drag_drop_item` / `drag_drop_item_into_group` / `drag_drop_selection` の state invariant を抽出する。
- import/save/auth: `login`、`save`、`import_json`、`group_regroup_json` を emulator smoke と JSON import unit/integration に統合する。
- UI contract: `add_kozane_dialog`、`adjust_font_size`、`scaled_fusen` は現行 state/API で書き直す。
- 削除: `group`、`many_fusen`、`transform`、`tutorial`、`visit_reset` は、根拠を残して旧 Movidea folder から外す候補にする。

## 2026-06-03 実装反映

上の分類をもとに、整理 pass を `work/kozaneba` に反映した。最終的に `cypress/e2e/movidea/` の legacy specs は 0 本になった。

Movidea から Kozaneba 側へ移したもの:

- `add_kozane_dialog.cy.ts` -> `cypress/e2e/kozaneba/test_add_kozane_dialog.cy.ts`
- `adjust_font_size.cy.ts` の render contract -> `cypress/e2e/kozaneba/test_adjust_font_size.cy.ts`
- `login.cy.ts` -> `cypress/e2e/kozaneba/test_login.cy.ts`
- `save.cy.ts` -> `cypress/e2e/kozaneba/test_save.cy.ts`
- `scaled_fusen.cy.ts` / `scaled_fusen_group.cy.ts` -> `cypress/e2e/kozaneba/test_kozane_scale.cy.ts`
- `selection.cy.ts` の hit test と selection -> make group -> `cypress/e2e/kozaneba/test_selection.cy.ts` へ追加

unit test へ移したもの:

- `group_regroup_json.cy.ts` -> `src/Regroup/importRegroupJSON.test.ts`
- `adjust_font_size.cy.ts` の algorithm 部分 -> `src/Kozane/AdjustFontSize.test.ts`
- `import_json.cy.ts` の legacy item upgrade 部分 -> `src/utils/piece_to_kozane.test.ts`
- `dimension.cy.ts`, `group_translated.cy.ts`, `nested_group.cy.ts` の geometry 部分 -> `src/dimension/item_layout.test.ts`
- `drag.cy.ts`, `drag_in_out.cy.ts`, `drag_group_to_group.cy.ts`, `drag_in_out_of_nested_group.cy.ts`, `selection.cy.ts` の state transition 部分 -> `src/Event/drag_drop_state.test.ts` と既存 `cypress/e2e/kozaneba/test_drag.cy.ts`

旧 Movidea folder から削除したもの:

- promote 済み: `add_kozane_dialog.cy.ts`, `group_regroup_json.cy.ts`, `login.cy.ts`, `save.cy.ts`, `scaled_fusen.cy.ts`, `scaled_fusen_group.cy.ts`
- delete 判定: `group.cy.ts`, `many_fusen.cy.ts`, `transform.cy.ts`, `tutorial.cy.ts`, `visit_reset.cy.ts`
- 第 2 pass で置換済み: `adjust_font_size.cy.ts`, `dimension.cy.ts`, `drag.cy.ts`, `drag_closed_group.cy.ts`, `drag_group_to_group.cy.ts`, `drag_in_out.cy.ts`, `drag_in_out_of_nested_group.cy.ts`, `group_translated.cy.ts`, `import_json.cy.ts`, `nested_group.cy.ts`, `selection.cy.ts`, `three_fusens.cy.ts`

残りの Movidea specs は 0 本。旧 direct HTML5 drag/drop と pixel exact DOM assertion は required gate から外し、守るべき挙動は現行 Kozaneba Cypress または unit/state test 側へ移した。

実行結果:

- `npm test`: 10 files / 23 tests pass
- `npm run build`: pass
- `npm run cypress:emulator-smoke`: 3 specs / 3 tests pass
- `npm run cypress:kozaneba-all`: 22 specs / 42 tests pass

## Sources

- [cypress/e2e/movidea](../../work/kozaneba/cypress/e2e/movidea)
- [cypress/e2e/kozaneba](../../work/kozaneba/cypress/e2e/kozaneba)
- [cypress/support/e2e.ts](../../work/kozaneba/cypress/support/e2e.ts)
- [scripts/cypress-emulator-smoke.sh](../../work/kozaneba/scripts/cypress-emulator-smoke.sh)
- [src/Global/exposeGlobal.ts](../../work/kozaneba/src/Global/exposeGlobal.ts)
- [src/Global/initializeGlobalState.ts](../../work/kozaneba/src/Global/initializeGlobalState.ts)
- [src/Canvas/ItemCanvas.tsx](../../work/kozaneba/src/Canvas/ItemCanvas.tsx)
- [src/Event/fast_drag_manager.ts](../../work/kozaneba/src/Event/fast_drag_manager.ts)
- [src/Event/drag_drop_item.ts](../../work/kozaneba/src/Event/drag_drop_item.ts)
- [src/Event/drag_drop_item_into_group.ts](../../work/kozaneba/src/Event/drag_drop_item_into_group.ts)
- [src/Event/drag_drop_selection.ts](../../work/kozaneba/src/Event/drag_drop_selection.ts)
- [src/Event/drag_drop_selection_into_group.ts](../../work/kozaneba/src/Event/drag_drop_selection_into_group.ts)
- [src/Event/drag_drop_state.test.ts](../../work/kozaneba/src/Event/drag_drop_state.test.ts)
- [src/Event/finish_selecting.ts](../../work/kozaneba/src/Event/finish_selecting.ts)
- [src/dimension/item_layout.test.ts](../../work/kozaneba/src/dimension/item_layout.test.ts)
- [src/dimension/world_to_screen.ts](../../work/kozaneba/src/dimension/world_to_screen.ts)
- [src/dimension/world_to_screen.test.ts](../../work/kozaneba/src/dimension/world_to_screen.test.ts)
- [src/Event/get_position_after_parent_change.test.ts](../../work/kozaneba/src/Event/get_position_after_parent_change.test.ts)
- [src/Event/get_total_offset_of_parents.test.ts](../../work/kozaneba/src/Event/get_total_offset_of_parents.test.ts)
- [src/Kozane/AdjustFontSize.tsx](../../work/kozaneba/src/Kozane/AdjustFontSize.tsx)
- [src/Kozane/AdjustFontSize.test.ts](../../work/kozaneba/src/Kozane/AdjustFontSize.test.ts)
- [src/Kozane/useAdjustFontsizeStyle.tsx](../../work/kozaneba/src/Kozane/useAdjustFontsizeStyle.tsx)
- [src/Regroup/importRegroupJSON.ts](../../work/kozaneba/src/Regroup/importRegroupJSON.ts)
- [src/Regroup/importRegroupJSON.test.ts](../../work/kozaneba/src/Regroup/importRegroupJSON.test.ts)
- [src/utils/JSON](../../work/kozaneba/src/utils/JSON)
- [src/utils/piece_to_kozane.test.ts](../../work/kozaneba/src/utils/piece_to_kozane.test.ts)
