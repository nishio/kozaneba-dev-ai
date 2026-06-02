---
title: Kozaneba テスト基盤調査 2026-06
type: theme
created: 2026-06-02
updated: 2026-06-02
sources:
  - work/kozaneba/package.json
  - work/kozaneba/netlify.toml
  - work/kozaneba/firebase.json
  - work/kozaneba/cypress.config.ts
  - work/kozaneba/cypress/support/e2e.ts
  - work/kozaneba/src/Cloud/init_firebase.ts
  - work/kozaneba/src/Global/exposeGlobal.ts
  - work/kozaneba/src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx
---

# Kozaneba テスト基盤調査 2026-06

## 要約

今回の [Plan B 試行 2026-06](Plan_B試行_2026-06.md) で分かった重要点は、辺ラベル実装そのものよりも、Kozaneba 本体のテスト基盤が新規実装を受け止める状態になっていないことだった。

`origin/main` 自体が Cypress 全件で落ちる。これは今回の実装変更で壊したというより、CI が Cypress を走らせていなかったため、壊れたテスト群が main に残っていたと見るのが自然である。

したがって、今後の新機能実装の前提は「機能追加」ではなく「再現可能なテスト実行環境の整備」になる。

## 確認した事実

### CI はアプリテストを実行していない

- `work/kozaneba` には `.github/workflows/` が存在しない。
- GitHub Actions に見えている workflow は GitHub 管理の `CodeQL` と `Dependabot Updates` のみで、`npm test` / Cypress / build test 用の workflow ではない。
- PR #36 の check は Netlify deploy preview 系だけだった。
- `main` branch protection も設定されていない。

### Netlify は build のみで test しない

`netlify.toml` の build command は以下。

```sh
CI=false npm install --legacy-peer-deps && npm run build
```

これは `npm test` も Cypress も実行しない。さらに `CI=false` により、Create React App の CI 扱いも避けている。

### Cypress 全件の現状

`origin/main` (`5de81c2`) を別 worktree + port 3001 で実行した結果:

```sh
env -u ELECTRON_RUN_AS_NODE npx cypress run --config baseUrl=http://localhost:3001,video=false
```

結果:

- `39 specs 中 20 specs failed`
- `50 tests 中 21 tests failed`

ローカル main (`898daf2`) は、追加した辺ラベル spec を含めて:

- `40 specs 中 20 specs failed`
- `55 tests 中 21 tests failed`

追加した `kozaneba/test_line_label.cy.ts` は通っている。つまり、今回の追加変更で失敗 spec 数が増えたわけではない。

### Cypress 実行環境の注意点

Cypress 12.4.0 は、この環境では `ELECTRON_RUN_AS_NODE=1` が残っていると起動に失敗する。実行時には以下のように環境変数を外す必要がある。

```sh
env -u ELECTRON_RUN_AS_NODE npx cypress run ...
```

この知見は `codex:preflight` に反映済み。

## 失敗の分類

### Firebase emulator 依存

該当例:

- `kozaneba/test_tutorial.cy.ts`
- `movidea/login.cy.ts`
- `movidea/save.cy.ts`
- `movidea/tutorial.cy.ts`

これらは Auth / Firestore emulator を前提にしている。`firebase.json` には emulator 設定がある。

- Auth: `9099`
- Firestore: `8080`
- Functions: `5001`

しかし調査時点では `firebase` CLI が入っておらず、9099 番の Auth emulator も起動していなかった。

また、Cypress spec 側には古い Firebase API 前提の記述が残っている。

```ts
import firebase from "firebase/app";
import "firebase/auth";
firebase.auth.GoogleAuthProvider.credential(...)
```

アプリ本体は `src/Cloud/init_firebase.ts` で `firebase/compat/app` / `firebase/compat/auth` を使っており、Cypress 側の import と前提がずれている。このため、emulator 起動以前に `firebase.auth` が undefined で落ちるケースがある。

### AddKozaneDialog の textarea 可視性

該当例:

- `movidea/add_kozane_dialog.cy.ts`
- `movidea/drag_closed_group.cy.ts`
- `movidea/drag_group_to_group.cy.ts`
- `movidea/save.cy.ts`

`AddKozaneDialog` の textarea が `position: fixed` 扱いになり、`DialogActions` に覆われて Cypress が入力できない。

これは単なるテスト都合ではなく、実 UI としても危うい。`DialogContent` / `TextareaAutosize` のレイアウトを見直し、textarea が dialog footer に覆われない構造にする必要がある。

### drag / pointer-events 系

該当例:

- `movidea/drag.cy.ts`
- `movidea/tutorial.cy.ts`

`fast_drag_manager.ts` はドラッグ中に対象 DOM へ `pointerEvents = "none"` を設定する。これは実操作では背景へイベントを通す意図だが、Cypress がその対象要素へ直接 `trigger()` しようとすると actionability check に落ちる。

この領域は「実 UI のイベントモデル」と「Cypress の直接 DOM 操作」が噛み合っていない。テスト helper 側で、実ユーザー操作に近い canvas 起点の mouse sequence に寄せるか、明示的に `{ force: true }` を使う箇所を限定する必要がある。

### pixel exact な座標期待値

該当例:

- `kozaneba/test_drag.cy.ts`
- `movidea/dimension.cy.ts`
- `movidea/drag_in_out.cy.ts`
- `movidea/drag_in_out_of_nested_group.cy.ts`
- `movidea/group_translated.cy.ts`
- `movidea/nested_group.cy.ts`
- `movidea/scaled_fusen.cy.ts`
- `movidea/selection.cy.ts`

`cypress/support/e2e.ts` の `hasPosition` は `getBoundingClientRect().x/y` を完全一致で比較している。

```ts
cr.x === x
cr.y === y
```

これは viewport、MUI、font rendering、React 18、styled-components、ブラウザ差分に弱い。UI レイアウトの正確性を見たい場合でも、許容誤差付き assertion、world coordinate 側の状態検証、または「相対的に移動したか」を見る assertion に分けるべきである。

### legacy movidea spec

`cypress/e2e/movidea/` は前身 Movidea 時代のテスト資産で、現在の Kozaneba UI/API との同期が取れていない箇所が多い。

これは価値がないという意味ではない。むしろ regression history として価値がある。ただし、現行 CI の必須 gate にそのまま全件を入れると、新規実装を始める前に失敗が山積みになる。

## Firebase emulator 導入について

Firebase emulator を入れて auth/save/tutorial 系をテストする方針は正しい。外部本番 Firebase に依存せず、保存・認証・ユーザー dialog まで確認できるようにする必要がある。

ただし注意点がある。

- 最新 `firebase-tools@15.19.0` は Node `>=20` 系を要求する。
- この repo の Netlify 設定は Node 16。
- Node 16 と衝突しにくい候補は `firebase-tools@12.9.1`。

2026-06-02 の作業中断直前に、`work/kozaneba` で `npm install --save-dev firebase-tools@12.9.1 --legacy-peer-deps` まで実行済み。ただし、これはまだ方針確定前の変更なので commit していない。

## 2026-06-02 の emulator 導入テスト結果

その後、`firebase-tools@12.9.1` を使って Auth / Firestore emulator を起動し、Cypress を実行した。

最初の `movidea/login.cy.ts` は emulator 自体は起動したが、Cypress spec 側が `firebase/app` + `firebase/auth` を使っており、アプリ本体の `firebase/compat/*` とずれていたため `firebase.auth` が undefined で失敗した。

compat import に揃えた後、`login` は Auth emulator 経由で pass。さらに `AddKozaneDialog` の textarea が small viewport で DialogActions に覆われる問題を修正した結果、`save` も Auth / Firestore emulator 下で pass した。

追加した smoke script:

```sh
npm run cypress:emulator-smoke
```

対象:

- `cypress/e2e/movidea/login.cy.ts`
- `cypress/e2e/movidea/save.cy.ts`
- `cypress/e2e/kozaneba/test_tutorial.cy.ts`

結果:

- `npm run cypress:emulator-smoke`: pass
- `npm run codex:preflight`: pass
- 全 Cypress with emulator: `40 specs 中 16 specs failed`, `55 tests 中 17 tests failed`

以前の全 Cypress は `40 specs 中 20 specs failed`, `55 tests 中 21 tests failed` だったので、Firebase emulator と AddKozaneDialog 修正で 4 spec / 4 test 分の失敗を剥がせた。

まだ残っている主な失敗は次の系統。

- `kozaneba/test_drag.cy.ts` の nested drag 座標期待値
- `movidea/` の legacy 座標期待値
- `pointer-events: none` 中の Cypress direct trigger
- `selection-view` / MUI menu の旧操作前提
- `visit_reset` の old API expectation

## 推奨する進め方

### 1. 必須 smoke test を CI に載せる

まずは壊れていない現行 Kozaneba の最小契約を gate にする。

- `npm test -- --watchAll=false`
- `npm run build`
- `#blank` で AppBar / canvas / StatusBar が出る
- 壊れた `localStorage.onLoad` でも boot が止まらない
- 辺ラベルなど、今回追加した対象 spec

### 2. Firebase emulator 付き test script を作る

Auth / Firestore を含む spec は emulator 起動付き script に分ける。

候補:

```sh
npx firebase emulators:exec --only auth,firestore \
  "env -u ELECTRON_RUN_AS_NODE npx cypress run --spec <auth/save/tutorial subset> --config video=false"
```

実際には dev server 起動、emulator 起動、Cypress 実行を一つの script にまとめる必要がある。

### 3. legacy spec を quarantine する

`movidea/` spec は削除せず、以下のように分類する。

- 現行 Kozaneba の必須挙動として直す
- emulator があれば直せる
- UI helper を更新すれば直せる
- 歴史的 regression として保留する

保留するものは `skip` ではなく、理由と復帰条件を明記した quarantine list に入れるのがよい。

### 4. UI test helper を整備する

必要な helper:

- `cy.visitBlank()` または `cy.visitKozanebaBlank()`
- `cy.assertBooted()`
- `cy.openMainMenuItem("Add Kozane")`
- `cy.addKozanes("a\nb\nc")`
- `cy.dragItemToCanvas(id, x, y)`
- `cy.dragItemIntoGroup(id, groupId)`
- `cy.signInWithEmulatorUser(...)`

個別 spec が MUI DOM 構造や pixel exact 座標に直接依存しないようにする。

## 含意

今回の失敗は「Codex が実装できない」だけではなく、「人間が見ても新規実装の前提となるテストの意味が曖昧になっている」ことを示している。

Plan B の仮説検証を続けるには、まず次を明文化する必要がある。

- どのテストが現行 Kozaneba の品質 gate なのか
- どのテストが legacy regression なのか
- Firebase emulator を含む integration test をどう起動するのか
- 白画面を見逃さない boot contract をどう CI に固定するのか

この基盤ができると、AI Agent に実装を投げる前に「最低限ここまでは自動で検証する」という境界が作れる。

## Sources

- [package.json](../../work/kozaneba/package.json)
- [netlify.toml](../../work/kozaneba/netlify.toml)
- [firebase.json](../../work/kozaneba/firebase.json)
- [cypress.config.ts](../../work/kozaneba/cypress.config.ts)
- [cypress/support/e2e.ts](../../work/kozaneba/cypress/support/e2e.ts)
- [src/Cloud/init_firebase.ts](../../work/kozaneba/src/Cloud/init_firebase.ts)
- [src/Global/exposeGlobal.ts](../../work/kozaneba/src/Global/exposeGlobal.ts)
- [src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx](../../work/kozaneba/src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx)
