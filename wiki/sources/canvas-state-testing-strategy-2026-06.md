---
title: キャンバス状態テスト戦略 2026-06
type: source
created: 2026-06-02
updated: 2026-06-02
sources:
  - raw/2026-06-02_キャンバス状態テスト戦略.md
  - work/kozaneba/src/Global/initializeGlobalState.ts
  - work/kozaneba/src/dimension/world_to_screen.ts
  - work/kozaneba/src/Event/onWheel.tsx
  - work/kozaneba/src/Event/fast_drag_manager.ts
  - work/kozaneba/cypress/support/e2e.ts
  - work/kozaneba/cypress/e2e/kozaneba/test_drag.cy.ts
  - work/kozaneba/cypress/e2e/kozaneba/test_zoom.cy.ts
---

# キャンバス状態テスト戦略 2026-06

## 要約

このソースの中心判断は、キャンバス UI の正しさを「ピクセルが一致するか」ではなく、「キャンバス上の状態、特に world 座標が正しいか」で見ることにある。

ピクセル単位の E2E / screenshot regression は、フォント、アンチエイリアス、DPR、OS、ブラウザ差分に弱い。したがって、主戦場は状態モデル、座標変換、hit test、drag interaction、undo / redo、persistence 変換の単体・統合テストに置き、E2E は pointer events / bounding box / DPR / IME / 保存復元などブラウザ依存の代表操作に絞る。

## 提案されているテスト分担

### 厚く見る層

- board state の reducer / command
- screen coordinate と world coordinate の相互変換
- zoomAtPoint が cursor 下の world point を保存する性質
- pan / zoom 後の hit test
- move / resize / select
- 複数選択移動
- undo / redo
- persistence の変換処理

### 中くらいに見る層

- interaction controller
- pointerDown / pointerMove / pointerUp のイベント列
- keyboard shortcut

### 薄く見る層

- Playwright / Cypress などのブラウザ E2E
- visual regression / screenshot comparison

E2E では、実際のユーザー操作のつながり、pointer capture、canvas bounding box、devicePixelRatio、IME、window resize、保存復元を代表シナリオとして見る。

## Kozaneba への対応

現行 Kozaneba の状態は `INITIAL_GLOBAL_STATE` に集約されており、キャンバス状態に相当する主要フィールドは以下。

- `itemStore` / `drawOrder`: こざね・グループなどの実体と重なり順
- `scale` / `trans_x` / `trans_y`: viewport
- `selected_items` / `selectionRange`: 選択状態
- `annotations`: 線などの関係表現
- `mouseState` / `drag_target` / `dragstart_position`: interaction 中の状態

座標変換は [world_to_screen.ts](../../work/kozaneba/src/dimension/world_to_screen.ts) にあり、`document.body.clientWidth/Height` と ReactN global の `scale/trans_x/trans_y` に依存している。これは今回のメモが言う「screen 座標と world 座標の変換がバグの温床」に直接当たる。

また、`onWheel.tsx` の `zoom_around_pointer` は「マウス位置の下にある world 座標が zoom 後もずれない」ことを狙う実装であり、性質ベースまたは表形式の unit test を置く価値が高い。

## 既存テストとの接続

現行 Cypress にはすでに `cy.getGlobal()` / `cy.setGlobal()` / `cy.updateGlobal()` があり、状態を見る入口は存在する。`window.__TEST_API__` を新設しなくても、development build の `window.movidea` が近い役割を持つ。

一方で [cypress/support/e2e.ts](../../work/kozaneba/cypress/support/e2e.ts) の `hasPosition` は `getBoundingClientRect().x/y` の完全一致を見ており、[test_zoom.cy.ts](../../work/kozaneba/cypress/e2e/kozaneba/test_zoom.cy.ts) などは DOM 座標の小数値に強く依存している。これは既存の [Kozaneba テスト基盤調査](../themes/Kozanebaテスト基盤調査_2026-06.md) で分類した pixel exact failure と同じ問題である。

[test_drag.cy.ts](../../work/kozaneba/cypress/e2e/kozaneba/test_drag.cy.ts) は良い面と悪い面を両方持つ。ドラッグ中は DOM style の一時変化も見ているが、mouseup 後には `itemStore["1"].position` を見ているケースもあり、「イベント列から状態変化を見る」方向に近い。今後は DOM の絶対 x/y よりも、world coordinate の差分、親子関係、選択状態、drawOrder を主 assertion に寄せるのが自然である。

## 設計上の含意

メモは、キャンバス UI を次のように分離することを推奨している。

```text
pointer / keyboard events
        ↓
interaction controller
        ↓
commands
        ↓
state reducer
        ↓
renderer
```

現行 Kozaneba では、`fast_drag_manager.ts` が drag 中に DOM style を直接変更し、mouseup 時に state を確定する構造になっている。これは操作の体感速度には効くが、テストの観点では interaction controller と renderer の境界が曖昧になる。

Plan B で全面的な設計変更をする必要はないが、次にテスト基盤を整えるなら、まず以下を抽出するのが現実的である。

- `screen_to_world` / `world_to_screen` を DOM 幅依存から切り離した pure helper
- `zoom_around_pointer` の計算部分を global state 更新から切り離した pure helper
- drag 後の world 座標差分を検証する helper
- `getBoundingClientRect()` 完全一致ではなく、state / world coordinate / 許容誤差付き DOM assertion を使う Cypress helper

## 次に活かす問い

- `window.movidea` をこのまま test API として育てるか、`window.__TEST_API__` 的な明示名に寄せるか
- `world_to_screen(screen_to_world(p)) ≒ p` を unit test に載せるなら、DOM サイズ依存をどう注入可能にするか
- `test_drag.cy.ts` の失敗を、DOM 座標期待値の修正ではなく「nested group の state invariant」として書き直せるか
- visual regression はどの固定ケースだけに絞るか
- IME / 日本語入力 / 長文 / 空文字 / paste は、E2E の代表シナリオとしてどこまで CI gate に入れるか

## Sources

- [2026-06-02_キャンバス状態テスト戦略](../../raw/2026-06-02_キャンバス状態テスト戦略.md)
- [initializeGlobalState.ts](../../work/kozaneba/src/Global/initializeGlobalState.ts)
- [world_to_screen.ts](../../work/kozaneba/src/dimension/world_to_screen.ts)
- [onWheel.tsx](../../work/kozaneba/src/Event/onWheel.tsx)
- [fast_drag_manager.ts](../../work/kozaneba/src/Event/fast_drag_manager.ts)
- [cypress/support/e2e.ts](../../work/kozaneba/cypress/support/e2e.ts)
- [test_drag.cy.ts](../../work/kozaneba/cypress/e2e/kozaneba/test_drag.cy.ts)
- [test_zoom.cy.ts](../../work/kozaneba/cypress/e2e/kozaneba/test_zoom.cy.ts)
