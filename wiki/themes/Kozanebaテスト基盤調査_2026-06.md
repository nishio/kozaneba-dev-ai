---
title: Kozaneba テスト基盤調査 2026-06
type: theme
created: 2026-06-02
updated: 2026-06-03
sources:
  - wiki/themes/CI安定化とVite移行_2026-06.md
  - raw/2026-06-02_キャンバス状態テスト戦略.md
  - wiki/sources/canvas-state-testing-strategy-2026-06.md
  - https://docs.cypress.io/app/core-concepts/retry-ability
  - https://playwright.dev/docs/actionability
  - https://github.com/microsoft/Webwright
  - https://webdriver.io/docs/why-webdriverio/
  - work/kozaneba/package.json
  - work/kozaneba/netlify.toml
  - work/kozaneba/firebase.json
  - work/kozaneba/cypress.config.ts
  - work/kozaneba/cypress/e2e/kozaneba/test_drag.cy.ts
  - work/kozaneba/cypress/e2e/kozaneba/test_zoom.cy.ts
  - work/kozaneba/cypress/support/e2e.ts
  - work/kozaneba/src/Cloud/init_firebase.ts
  - work/kozaneba/src/dimension/world_to_screen.ts
  - work/kozaneba/src/Event/onWheel.tsx
  - work/kozaneba/src/Event/fast_drag_manager.ts
  - work/kozaneba/src/Event/drag_drop_item.ts
  - work/kozaneba/src/Event/drag_drop_item_into_group.ts
  - work/kozaneba/src/Event/get_total_offset_of_parents.ts
  - work/kozaneba/functions/src/index.ts
  - work/kozaneba/src/Scrapbox/add_scrapbox_links.ts
  - work/kozaneba/src/Group/calc_closed_style.tsx
  - work/kozaneba/package-lock.json
  - work/kozaneba/yarn.lock
  - work/kozaneba-drag-investigation/src/Event/drag_drop_item_into_group.ts
  - work/kozaneba-drag-investigation/src/Event/get_total_offset_of_parents.ts
  - work/kozaneba-drag-investigation/cypress/e2e/kozaneba/test_drag.cy.ts
  - work/kozaneba/src/Global/exposeGlobal.ts
  - work/kozaneba/src/API/KozanebaAPI.ts
  - work/kozaneba/src/API/run_user_script.ts
  - work/kozaneba/src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx
---

# Kozaneba テスト基盤調査 2026-06

## 2026-06-03 更新

このページの前半にある「CI がアプリテストを実行していない」「Netlify は build のみで test しない」「CRA 移行が必要」という記述は、2026-06-02 調査時点の状態である。

その後、PR #40〜#44 で、座標ロジック単体テスト、Kozaneba required CI、Node 24、Vite 移行、Vite 環境 API の後処理まで完了した。現在の実施記録と学びは [CI安定化とVite移行 2026-06](CI安定化とVite移行_2026-06.md) に保存した。

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

## Security alert 修正と CRA 移行の必要性

2026-06-02 に GitHub security alert 対応として、Code scanning と Dependabot の両方を確認した。

Code scanning 側は、以下の実装修正で open alert 0 件になった。

- `functions/src/index.ts`: `get_scrapbox_page` は任意 URL fetch ではなく、`https://scrapbox.io` の URL だけを受け付け、固定 origin の Scrapbox API URL に変換する。旧 `proxy` は任意 URL fetch を停止し `410` を返す。
- `src/Scrapbox/add_scrapbox_links.ts`: `startsWith("https://scrapbox.io")` ではなく `URL` parse 後に `protocol` と `hostname` を検証する。
- `src/Group/calc_closed_style.tsx`: `.replace("\n", " ")` を `.replace(/\n/g, " ")` にし、複数改行をすべて処理する。

Dependabot 側は大量の alert を依存更新と lockfile 更新で削減したが、最終的に 6 件だけ残った。残りはすべて `react-scripts > webpack-dev-server` 由来の medium alert で、`package-lock.json` に 3 件、`yarn.lock` に 3 件である。

根本原因は、現行 Kozaneba が Create React App / `react-scripts@5.0.1` に依存していること。`webpack-dev-server` の alert を消すには advisory 範囲外の `webpack-dev-server@5.2.4` へ上げる必要があるが、単純に npm `overrides` / Yarn `resolutions` で強制すると `npm start` が壊れる。実際に試すと CRA 側が渡す `onAfterSetupMiddleware` / `onBeforeSetupMiddleware` が webpack-dev-server 5 の schema で拒否され、dev server 起動時に `Invalid options object` で停止した。

したがって、この 6 件を安全に消すには「推移依存を上書きする」だけでは足りない。選択肢は次のどれかになる。

- CRA / `react-scripts` から Vite などへ移行する。
- `react-scripts` を eject / fork / patch して webpack-dev-server 5 の `setupMiddlewares` 形式に対応する。
- 短期的には、production build ではなく dev server 由来の medium alert として扱い、dev server を外部公開しない運用でリスクを限定する。

これは「今すぐ Kozaneba の本番挙動を壊している問題」ではないが、Dependabot を完全に緑にするには避けて通れない開発基盤負債である。今後 CI を整備する時にも、CRA に留まるか、Vite 等へ移行するかを明示的に判断する必要がある。

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

`kozaneba/test_drag.cy.ts` の失敗は、Movidea legacy ではなく Kozaneba 側にも残る failure である。`cypress/e2e/kozaneba/*.cy.ts` だけを Firebase emulator 付きで実行すると `17 specs 中 1 spec failed`, `30 tests 中 1 test failed` で、落ちるのは `bug fix: drag out from nested groups(drag G1 in/out)` のみだった。

同じ spec を `origin/main` (`5de81c2`) の別 worktree + port 3001 でも実行し、同じ `expected x:25 is 225` failure を確認した。したがって、この失敗は 2026-06-02 のローカル変更で混入したものではなく、少なくとも PR #36 merge 時点の `origin/main` に既に存在する。

ただし、これは「最初から無価値なテスト」ではない。該当ケースは 2021-12-27 の `refactor tests` 由来で、nested group から group を出し入れした後に内部 kozane の座標が壊れないことを見る回帰テストである。2023-01-27 の `ignore some tests` では隣接する `drag G2` と `closed group in another group` はコメントアウトされたが、この `drag G1 in/out` は残されていた。つまり、現行 UI で本当に不要になったと判断するまでは、基本的な nested drag regression として扱うべきである。

### `test_drag` failure の bisect 結果

`891f36d` (`ignore some tests`, 2023-01-27) は `test_drag.cy.ts` 7 tests が全通する。`origin/main` (`5de81c2`) は同じ spec の `drag G1 in/out` で落ちる。

手動 bisect の結果、通常の checkout + lockfile 固定で最初に再現可能な bad commit は `ceea496` (`依存パッケージの更新: yarn.lockとpackage-lock.jsonを追加`) だった。この commit 自体は lockfile 追加・更新のみで、ソースコードは触っていない。

その直前の範囲では `6f440f4` (`依存パッケージの更新: React 17→18、Firebase 8→9`) から `3785e91` までが、`package.json` と `yarn.lock` の不整合により frozen install できず skip になった。そこで `6f440f4` を別 worktree で non-frozen `yarn install` して確認すると、同じ `expected x:25 is 225` failure が再現した。

したがって、実質的な退行は `6f440f4` の大規模依存更新に入ったと見るのが妥当である。差分には React 17→18、ReactDOM.render→`createRoot`、styled-components 5→6、MUI 5 alpha→beta、Firebase 8→11 系への移行が含まれる。今回の bisect だけでは React 18 / styled-components / MUI のどれが座標差分の直接原因かまでは分離していないが、Firebase 変更単独ではなく、フロントエンド依存・描画レイヤの更新に伴う nested drag 座標 regression と見るべきである。

### 公開版での確認可否

`https://kozaneba.netlify.app/#blank` は 2026-06-02 時点で HTTP 200 を返す。ただし、既存の `test_drag.cy.ts` を公開 URL に向けても、production build では `window.movidea` が存在しないため、全ケースが test hook 不在で失敗する。これは `window.movidea` が `exposeGlobalForTest()` 経由で development build のみ公開されるためで、公開版の drag 挙動そのものを判定した結果ではない。

公開版でも `window.kozaneba` は公開 API として存在するので、`try_to_import_json` と UI 操作を組み合わせた一時 Cypress check も試した。しかしこの check は既知 bad のローカル main でも通ったため、`drag G1 in/out` regression の検出器としては使えない。

したがって、現状の公開版がこの nested drag regression を実際に踏むかは、既存自動テストだけでは断定できない。確認するなら、人間が実 UI で「ネストした group を作る」「内側 group を外へ出して、外側 group に戻す」「中の kozane が横に飛ばないか」をピンポイントで見るのが最短である。

### 人間テスト観察: `A(B(C))`

nishio の実 UI 確認では、`A(B(C))` というネスト group を作り、`C` を root にドラッグした後、`A` の sibling として扱う場合は自然だが、`B` の sibling にした時には位置が不自然になる。

これは `test_drag.cy.ts` の失敗が単なる古い pixel exact assertion ではなく、「root に出す経路」と「group 内の sibling に戻す経路」で座標変換の扱いが違うという現行 UI 上の問題である可能性を強める。

コード上の分岐としては、root への drop は `drag_drop_item.ts` で `get_total_offset_of_parents(parent, g)` を足している。一方、group への drop は `drag_drop_item_into_group.ts` で、旧親がある場合は旧親 offset と新親 offset を使うが、root から group に入れる場合は `group_draft.position` だけを引いている。また、`get_total_offset_of_parents.ts` は `g` を受け取る一方で、親探索には `find_parent(current_parent)` を state 引数なしで呼んでおり、`updateGlobal` の draft state と現在 state が混ざる余地がある。ここは次に test を状態ベースで切る時の優先調査点である。

### clean worktree での修正結果

`work/kozaneba` が dirty だったため、`work/kozaneba-drag-investigation` を detached clean worktree として作成し、そこで修正・検証した。

修正内容:

- `get_total_offset_of_parents.ts` の親探索で `find_parent(current_parent, g)` を使い、offset 計算を渡された state にそろえた。
- `drag_drop_item_into_group.ts` の root → group branch で、`group_draft.position` だけではなく `get_total_offset_of_parents(group_id, g)` を引くようにした。これにより、root に出した group を nested group に入れる時も、親 chain 全体の offset を差し引く。
- `test_drag.cy.ts` に、offset を持つ `A(B(C))` 相当の構造で root の `C` を nested `B` に入れる回帰テストを追加した。
- 既存 `drag G1 in/out` は、React 18 以後の描画タイミングに合わせ、1回目の root drop 後に `G1` が root 位置へ描画されたことを待ってから次の drag を始めるようにした。

確認結果:

- `npm test -- --watchAll=false`: pass
- `npm run build`: pass
- `cypress/e2e/kozaneba/test_drag.cy.ts`: `8 tests` pass
- Firebase emulator 付き `cypress/e2e/kozaneba/*.cy.ts`: `17 specs`, `31 tests` pass

その後、PR [nishio/kozaneba#39](https://github.com/nishio/kozaneba/pull/39) は merge され、deploy 環境でも nishio が実 UI で動作確認した。つまり今回の `A(B(C))` 系 nested drag の不自然な位置ずれは、コード上の親 chain offset 修正で実際に直せたと判断できる。

### 今回うまくいったアプローチ

この修正では、最初から既存 `test_drag.cy.ts` の pixel exact failure だけを信じて修正しなかったことが効いた。

有効だった流れ:

- dirty な `work/kozaneba` から離れ、clean worktree で再現・修正・検証した。
- `A(B(C))` を人間が実 UI で触って観察し、「root に出すと自然だが nested sibling に戻すと不自然」という具体的な操作差分に落とした。
- 既存の `drag G1 in/out` failure を、React 18 後の描画タイミング問題と、実際の nested offset バグに分解した。
- root → nested group の座標変換を、DOM 座標だけでなく parent chain / state の観点から読み直した。
- 修正後に `test_drag` 単体だけでなく、Firebase emulator 付きの Kozaneba 系 Cypress 全体を回した。
- 最後に deploy 環境で人間が実 UI を確認した。

特に重要なのは、「Cypress が落ちているから実装が壊れている」とも「古い spec だから無視でよい」とも決めつけず、人間観察で操作の意味を補い、state / parent chain の不変条件へ変換した点である。キャンバス UI の regression では、pixel exact assertion は症状の検出器として使い、原因特定と恒久テストは world/state 側に寄せるのがよい。

## 今回の学び

### 「テストが通る」の主語を分ける

「テストが通る」と言うときは、必ずどの gate を指しているかを明示する必要がある。

- `npm test -- --watchAll=false`
- `npm run build`
- `npm run codex:preflight`
- `npm run cypress:emulator-smoke`
- 全 Cypress

今回、対象 spec と unit/build が通ったことを「全部通っている」と誤解しうる状態があった。これは実装品質以前にコミュニケーションの失敗である。今後は、通した gate と落ちている gate を同時に報告する。

### CI がないと main は自然に壊れる

`origin/main` の Cypress が大量に落ちていたのは不自然ではない。CI が Cypress を走らせていなければ、壊れた spec は merge を止めない。つまり「main がテストに落ちる」こと自体より、「落ちることを merge 時に検出していない」ことが根本問題だった。

次にやるべきことは、全 Cypress をいきなり必須にすることではなく、現時点で通る smoke gate を CI に固定すること。

### Firebase emulator 不在だけが原因ではない

Auth / Firestore emulator を入れる方針は正しかったが、単に emulator を起動するだけでは不十分だった。

実際には次の問題が順に露出した。

- `firebase-tools` が未導入
- Cypress spec の Firebase import がアプリ本体の compat import とずれていた
- Auth emulator は最初の network call 前に接続しないと失敗する
- 旧 spec は `NISHIO_TEST` のような現行 UI と合わない表示名を期待していた

したがって、emulator 導入は「外部依存をローカル化する」だけではなく、「テストとアプリ本体の Firebase API 前提を揃える」作業でもある。

### UI テストの失敗はテストだけの問題とは限らない

`AddKozaneDialog` の textarea は Cypress が入力できないだけでなく、小さい viewport で実 UI としても DialogActions に覆われうる構造だった。Cypress の actionability failure は、実装側のレイアウト欠陥を示すことがある。

今回の修正では、MUI `TextareaAutosize` の clone/高さ計算に依存せず native textarea + viewport 相対の高さ制約にした。これはテストを通すためだけでなく、実 UI の安定性にも寄与する。

### Cypress の direct trigger は実ユーザー操作ではない

`pointer-events: none` 中の DOM に `cy.trigger()` する失敗は、アプリのドラッグ実装と Cypress の direct DOM 操作が噛み合っていないことを示す。

この種の spec は、安易に `{ force: true }` を足す前に目的を分ける必要がある。

- 実ユーザー操作の再現をしたいなら、canvas 起点の mouse sequence helper に寄せる。
- 内部状態遷移だけを検証したいなら、DOM actionability を避けて global state / API を見る。
- 歴史的 regression を保持したいだけなら、legacy quarantine に入れる。

### pixel exact assertion は最後の手段にする

`getBoundingClientRect().x/y` の完全一致は、UI regression を検出できる一方で、font / browser / React / MUI / styled-components の差分に弱い。現行の全 Cypress 失敗の多くはこの系統だった。

今後は、座標検証を次に分ける。

- world coordinate の内部状態を検証する
- DOM 座標は許容誤差付きで検証する
- 絶対座標ではなく移動量・包含関係・順序を検証する
- pixel exact が必要な spec だけ明示的に残す

### 状態モデルをテストの主対象にする

[キャンバス状態テスト戦略 2026-06](../sources/canvas-state-testing-strategy-2026-06.md) の中心は、「付箋がそこに見えているか」ではなく「付箋の world 座標が正しいか」を基本の正解にすること。

Kozaneba では `INITIAL_GLOBAL_STATE` に `itemStore` / `drawOrder` / `scale` / `trans_x` / `trans_y` / `selected_items` / `selectionRange` / `annotations` があり、すでに Cypress から `cy.getGlobal()` / `cy.setGlobal()` / `cy.updateGlobal()` で触れる。したがって、既存の `window.movidea` は `window.__TEST_API__` に近い役割をすでに持っている。

次に強化すべきなのは、見た目のピクセルではなく以下の state invariant である。

- `screen_to_world` と `world_to_screen` が互いにほぼ逆写像になる
- `zoom_around_pointer` 後も cursor 下の world point がずれない
- pan / zoom 後も selection / hit test がずれない
- 単体 drag 後は対象 item の world 座標だけが期待差分で変わる
- 複数選択 drag 後は全 selected item が同じ world 座標差分で動く
- nested group の出し入れ後も、子 item の表示位置と親子関係が矛盾しない
- `drawOrder` / annotation の重なり順や click 対象が state と一致する

これは現行 failure の扱いにも影響する。`test_drag.cy.ts` の nested drag failure は単に `[225, 225]` という DOM 座標期待値を直す問題ではなく、nested group 操作後の state invariant として書き直す候補である。

### イベント列は interaction controller として見る

ドラッグや選択は、全てをブラウザ E2E に寄せるのではなく、まず「pointerDown / pointerMove / pointerUp のイベント列から state がどう変わるか」として見るのが安定する。

現行 Kozaneba は `fast_drag_manager.ts` が drag 中の DOM style を直接動かし、mouseup 時に ReactN state を確定する。これは体感速度のための実装だが、テストでは次を分けて見る必要がある。

- drag 中の一時 DOM style は必要最小限だけ確認する
- mouseup 後の正しさは `itemStore` / `selected_items` / 親子関係 / `selectionRange` で確認する
- Cypress の direct `trigger()` と実ユーザー操作を混同しない
- 必要なら canvas 起点の mouse sequence helper と、state reducer 相当の unit test に分ける

この分担にすると、renderer の visual regression は「状態が明らかに描画されているか」を見る薄い層にできる。visual regression を使う場合も、空の場、数枚のこざね、選択中、複数選択中、zoom / pan 後、日本語テキスト、長文、重なり、程度の固定ケースに絞るのがよい。

### Cypress / Playwright / Webwright の使い分け

2026-06-02 の相談時点での判断は、**Kozaneba では Cypress から Playwright へ全面移行するより、まず state / world 座標の unit・integration test を厚くする**こと。

Cypress は既存資産があるため短期の正解である。`cy.getGlobal()` / `cy.setGlobal()` / `cy.updateGlobal()` を使えば、DOM の見た目ではなく ReactN state を見られる。ただし Cypress の強みである retry-ability は DOM query / assertion の文脈で効くものであり、`trigger()` による direct DOM 操作や `getBoundingClientRect()` 完全一致に寄せると、Kozaneba の drag / pointer-events / pixel exact failure には弱い。

Playwright は、新規に E2E を組むなら有力である。auto-waiting / auto-retrying assertion、test ごとの isolated browser context、parallel 実行が強く、実ブラウザ操作を少数の代表 scenario として CI に載せる用途に向く。特に IME、日本語入力、window resize、pointer 操作、複数 tab、保存復元のような「ブラウザ実装そのものを見たい」ケースでは Cypress より第一候補になりうる。

Webwright は、CI の決定的 test runner ではなく、Playwright を下回りで使う AI browser agent framework として見るべきである。探索、再現手順生成、テスト案生成、AI Agent に「この UI を触って問題を探す」作業をさせる補助には向くが、Kozaneba の品質 gate に直接置くものではない。

もし「Webwright」が WebdriverIO の意味なら、WebdriverIO は WebDriver / WebDriver BiDi / mobile / native / desktop まで広げたい場合の候補である。ただし Kozaneba は現時点では Web アプリの canvas / DOM interaction が中心なので、導入理由は Playwright より弱い。

したがって推奨は次の順序。

1. Jest などで `screen_to_world` / `world_to_screen` / `zoom_around_pointer` / nested group offset / selection / drag-drop state invariant を unit・integration test 化する。
2. 既存 Cypress は捨てず、boot / Firebase emulator smoke / 代表 regression の薄い gate として整理する。
3. Playwright は、Cypress 置換ではなく「本物のユーザー操作らしさ」が必要な少数 E2E から試験導入する。
4. Webwright は、AI に探索・再現・テスト案生成をさせる補助道具として扱い、CI の合否判定には使わない。

### AI Agent に投げる前に gate を固定する

AI Agent に新機能を投げる前に、少なくとも以下を通すべきである。

```sh
npm run codex:preflight
npm run cypress:emulator-smoke
```

この 2 つが通っていない状態で新機能を始めると、実装ミスと環境不備と legacy spec failure が混ざる。人間に画面確認を求める前に、自動 gate で「白画面ではない」「保存/認証の smoke が通る」ことを確認する。

### 次の一手

次の実装作業は CI の追加。

`.github/workflows/test.yml` を作り、まず次を走らせる。

- install
- `npm run codex:preflight`
- `npm run cypress:emulator-smoke`

全 Cypress はまだ必須 gate にしない。残り 16 failing specs は quarantine list を作って、現行 Kozaneba の必須挙動と legacy regression を分けてから扱う。

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
- [2026-06-02_キャンバス状態テスト戦略](../../raw/2026-06-02_キャンバス状態テスト戦略.md)
- [キャンバス状態テスト戦略 2026-06](../sources/canvas-state-testing-strategy-2026-06.md)
- [Cypress Retry-ability](https://docs.cypress.io/app/core-concepts/retry-ability)
- [Playwright Auto-waiting](https://playwright.dev/docs/actionability)
- [microsoft/Webwright](https://github.com/microsoft/Webwright)
- [WebdriverIO](https://webdriver.io/docs/why-webdriverio/)
- [cypress/support/e2e.ts](../../work/kozaneba/cypress/support/e2e.ts)
- [src/dimension/world_to_screen.ts](../../work/kozaneba/src/dimension/world_to_screen.ts)
- [src/Event/onWheel.tsx](../../work/kozaneba/src/Event/onWheel.tsx)
- [src/Event/fast_drag_manager.ts](../../work/kozaneba/src/Event/fast_drag_manager.ts)
- [src/Cloud/init_firebase.ts](../../work/kozaneba/src/Cloud/init_firebase.ts)
- [src/Global/exposeGlobal.ts](../../work/kozaneba/src/Global/exposeGlobal.ts)
- [src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx](../../work/kozaneba/src/Dialog/AddKozaneDialog/AddKozaneDialog.tsx)
- [functions/src/index.ts](../../work/kozaneba/functions/src/index.ts)
- [src/Scrapbox/add_scrapbox_links.ts](../../work/kozaneba/src/Scrapbox/add_scrapbox_links.ts)
- [src/Group/calc_closed_style.tsx](../../work/kozaneba/src/Group/calc_closed_style.tsx)
- [package-lock.json](../../work/kozaneba/package-lock.json)
- [yarn.lock](../../work/kozaneba/yarn.lock)
