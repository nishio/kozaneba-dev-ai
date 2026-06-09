---
title: 線UI 再設計 2026-06 — Codex 発注 prompt
type: theme
created: 2026-06-04
updated: 2026-06-04
sources:
  - wiki/themes/線UI再設計_2026-06.md
  - wiki/themes/AI委託の設計と検証_2026-06.md
  - wiki/themes/Plan_B試行_2026-06.md
  - wiki/concepts/線を引く機能.md
  - wiki/concepts/辺ラベル.md
  - wiki/sources/kozaneba-code-architecture.md
---

本ページは [線UI 再設計 2026-06](線UI再設計_2026-06.md) を Codex (cloud) への発注 prompt として再整形したもの。**そのまま全文を Codex タスクの prompt として貼り付けて使う**。

[AI 委託の設計と検証 2026-06](AI委託の設計と検証_2026-06.md) の spec template に従って、hard constraints / 設計史抜粋 / 自動テスト制約 / 完了条件 / 報告必須項目 を凝集してある。

---

## あなた(Codex)へのタスク

Kozaneba(`nishio/kozaneba` リポジトリ)の **線生成 UI 全体** を再設計実装する。前回の辺ラベル単独実装(`9e7122b Add inline line label editing`)は自動テスト pass / merge されたが、人間が実ブラウザで触ると double-click が動かず、設計史にある drag 不変条件と衝突することが判明した。本タスクはその根本問題を線生成 flow の再設計として解く。

PR は 1 本にまとめてよい。**自動テストが pass しただけでは完了ではない**。完了条件の頂点は「人間が dev server で実ブラウザを立ち上げて思考フローを止めずに線を引きラベルを付けられること」。

## 0. Hard constraints (Kozaneba 固有、絶対に破らない)

以下は Kozaneba の設計史で確定済みの原則。実装方針が衝突する場合は **実装前に PR description で問題提起** し、勝手に破らない。

1. **kozane body の drag は移動**(常に、例外なし)
   - 「線にクリック判定をつけて設定メニューを出す」過去試行は **「こざねを動かす」が妨げられた** ために 2022 年に撤回されている(後述 §3 参照)
2. **default で線が増えない方を選ぶ**
   - 「デフォルトで線が増える仕様と、増えない仕様とを選べる場合、増えない方を選ぶ。人間は線が多すぎると混乱する」(2023-02-27 フォーラム明文化、後述 §3 参照)
3. **モードレス志向**
   - toolbar での明示的モード突入はできるだけ避ける。新動線は hover anchor 起点で、既存 toolbar の line mode と排他しない
4. **mainstream UI に装飾を増やしすぎない**
   - 線に常時表示の装飾要素は増やさない(passive 状態は今と同じ静かさ)。affordance は hover で薄く表示するに留める
5. **iPad はセカンドプライオリティ**
   - まず PC マウスで完成。touch / pointer 対応は本 PR の scope 外(後続 PR)

## 1. Scope

### In scope

- 線生成 flow の再設計(hover anchor + drag + release)
- release 後の編集 window(label 入力 / 線種 chip / endpoint handle / 削除)
- 離脱 = commit ルールの統一実装
- passive 線への再編集復帰(ラベル text click / 中点 affordance)
- 既存の `9e7122b` で導入された inline label editing は **本再設計に置き換え**。passive 線の中点 affordance 経由の編集 window 復帰として再利用してよい(コードの完全削除は不要)
- 関連する Cypress / unit テスト の再整備(後述 §6 参照)

### Out of scope

- **N項関係 UI**(`items.length > 2`)。editing window から endpoint を追加する動線は将来拡張。今回は 2 項のみ
- **iPad / touch 対応**。hover が無い環境の代替 gesture は別 PR
- **AI による線・ラベル提案**。本タスクは UI 層のみ
- **既存 toolbar の line/arrow/double_heads/double_lines/delete ボタン廃止**。後方互換のため残置(段階的縮小は別 PR で扱う)
- **データモデル(`RTLineAnnot`)の schema 変更**。既存フィールド(`label`, `items`, `heads`, `is_doubled`)で完結する

判断に迷う scope 拡張は **やらずに PR description に書き残す**(spec writer 側で次の PR に切る)。

## 2. 中心アイデア(目指す UX)

> **kozane に hover → 縁の anchor を drag → 別 kozane で release で線確定 → release 後は「編集 window」に入る → 任意のアクション(別 click / scroll / hotkey / 等)で離脱 = commit**

### 2.1 描画フロー: hover anchor + drag

- kozane に hover すると **縁または周囲に小さな anchor 表示**
  - 視覚は控えめ(Kozaneba の「装飾を増やさない」哲学に合わせて Miro より控えめ)
  - 4 方向の小点 / 周囲 ring / 控えめな `+` / hover 中だけ薄く表示、のいずれか — 詳細はあなたの裁量だが、**default で見えない**ことを満たす
- anchor から drag で線描画開始
  - drag 中、ターゲット候補 kozane を highlight
  - kozane body の通常 drag(移動)とは **start 位置で完全に区別** = drag 不変条件維持
- 別 kozane で release → 線確定 → 編集 window に入る
- 空中で release → 何も起きない(default で線が増えない方を選ぶ)

### 2.2 編集 window(post-release, pre-commit)

release 直後、線は active な編集状態に留まる。表示する UI:

```
[kozane A]━━━━━●━━━━━[kozane B]
              │
        ┌─────┴─────┐
        │ [✎ label]  │   ← inline caret(autofocus)
        │ ─ → ↔ = 🗑 │   ← 線種 chip(現 type を highlight、🗑 = 削除)
        └───────────┘
   ●               ●
   (endpoint handle)
```

このウィンドウ中:

- **キーボード入力** → label に流れる(空のまま離脱すれば label 無し)
- **線種 chip の click** → 線種を切替(`heads[]` と `is_doubled` の組合せを書き換え)
  - 線種は §4.1 の対応表で表現
- **endpoint handle の drag** → 別 kozane へ drop で endpoint 差し替え = 相手間違い修正
- **🗑 click** → 線を消す(誤って引いた場合の即取消)

ホットキー(rotate / spread / 他)は編集中 **無効化**(既存 `disableHotKey` の仕組みを流用)。

### 2.3 離脱 = commit ルール(統一)

「離脱イベント = 編集 window を抜けて passive 状態へ確定」を **一つのルール** として定義する。

**commit する離脱イベント:**

- 別の kozane / 線 / 空間を click
- 別の hover anchor から drag を始める(= 次の線の描画開始)
- kozane body の drag(= 移動の開始)
- canvas pan / scroll / wheel / pinch zoom
- toolbar / menu の任意の操作
- Esc / Enter
- 編集対象外のホットキー入力

**commit しない(編集状態に留まる)アクション:**

- caret に文字入力 / Backspace / 矢印キー
- 線種 chip の click
- endpoint handle の drag(retarget 中)
- ラベル text / chip / handle / editor 自体の click
- マウスを動かすだけ(hover、無 click)

「キャンセル」semantics は持たない。離脱は常に commit。やり直しは:

- 編集 window 中 → 🗑 chip で削除
- 編集 window 後 → **Cmd+Z (undo)** で線創出ごと巻き戻す(既存 undo に乗る)

### 2.4 passive 線への再編集(commit 後)

| 線の状態 | 編集 window 復帰の起点 |
|---|---|
| **ラベルあり** | ラベル text を click → 同じ編集 window へ |
| **ラベルなし** | 線の **中点に最小 affordance**(4x4 px の控えめな点)→ hover で 20x20 hit zone に拡張 → click で編集 window へ |

線の中点は構造的に kozane と kozane の間の空間にあるため、小さな中点 affordance は **drag 不変条件をほぼ完全に維持**する(Miro / FigJam も同様の妥協を採用)。

復帰先の UI は §2.2 と同一。

## 3. 関連設計史(実装前に必ず読むこと、本文に抜粋を貼る)

以下は Kozaneba の過去判断。これらに反する実装方針を選ぶ場合は事前に PR description で問題提起する。

### 3.1 線にクリック判定 → kozane drag を妨害(2022-05、撤回済)

公開フォーラム `kozaneba-forum/Scrapbox_Integration.md`(2022-05-26)で nishio が明示:

> 以前、線に当たり判定をつけて設定メニューが出るようにしようとしたことがあって、これも「こざねを動かすこと」の妨げになったのでやめました.

含意:**線の全長 hit area は NG**。前回の Codex 実装 `9e7122b` は 16px hit zone を入れたが、これが本原則と衝突して結果的にノーオペになった。本再設計の「中点 affordance」「ラベル text 自体の click」は、構造的に kozane 間の空間にしか affordance を置かないことで本原則を守る設計。

### 3.2 SVG layer の `pointerEvents: "none"`(意図的設計)

[`src/Canvas/Annotation/AnnotationLayer.tsx`](../../work/kozaneba/src/Canvas/Annotation/AnnotationLayer.tsx) は SVG root 全体に `pointerEvents: "none"` を CSS で指定して背景に event を素通しさせている。これは §3.1 の含意を実装で担保するための **意図された設計**。

含意:

- AnnotationLayer の root の `pointerEvents: "none"` を **緩めない**
- 個別 affordance(anchor / 中点 dot / label text / chip / handle)が **CSS** で `pointerEvents: "auto"` を立てて個別に hit を取る形にする
- 前回の `9e7122b` は SVG attribute (`pointerEvents="stroke"`) を使ったが、現代ブラウザでは親要素 CSS が子要素の SVG attribute を上書きするため無効化された。**CSS で立てる**こと

### 3.3 「default で線が増えない方を選ぶ」(2023-02-27 明文化)

公開フォーラム `kozaneba-forum/Remove_Split-Kozane_feature.md`(2023-02-27):

> デフォルトで線が増える仕様と、増えない仕様とを選べる場合、増えない方を選んでいると言えます。人間は線が多すぎると混乱するので、増えない方が良いと思っています。

含意:

- 空中 release は **線を作らない**(2.1 の「空中で release → 何も起きない」)
- anchor から drag を始めただけで release していない時点では線は無い
- 誤って引いた線は 🗑 chip 一発で消える(編集 window 中)

### 3.4 線への直接イベントは未解決問題と nishio が認めている

`線を引く機能` wiki(概念ページ):

> 「こざねを動かす」が最頻出の操作なので、`AnnotationLayer` の `pointerEvents: "none"` は意図された設計であり、辺ラベル / 辺メニューの実装はこの根本制約と衝突する。「辺をクリックする」と「こざねをドラッグする」をどう両立するかは未解決問題のまま。

本再設計の解は「**線の中点に置く最小 affordance + label text 自体への click**」のみ hit を取り、線の全長には hit を取らない。これにより両立する。

### 3.5 「描いた瞬間 modal dialog」アンチパターン

2025-09 Devin 実装(PR #36)は線を引いた瞬間に label 入力 dialog を出して、ウザいと不採用になっている。本再設計は dialog ではなく **inline caret** で同等機能を満たす(modal が立ち上がらない、入力しなくても OK、思考フローを止めない)。

## 4. データモデルと状態

### 4.1 既存スキーマ(`src/Global/TAnnotation.ts`)— 変更なし

```ts
RTLineAnnot {
  label: String.optional()   // 2025-09 で追加済
  items: Array(RTItemId)     // 2 項のみ(本 PR は items.length === 2 のみ扱う)
  heads: Array(RTArrowHead)  // ["none"|"arrow", "none"|"arrow"]
  is_doubled: Boolean
  // ... 他既存フィールド
}
```

線種は以下の組合せで全て表現:

| ユーザから見た線種 | データ表現 |
|---|---|
| 線(無向) | `heads: ["none", "none"]`, `is_doubled: false` |
| 矢印(片方向) | `heads: ["none", "arrow"]`, `is_doubled: false` |
| 双方向矢印 | `heads: ["arrow", "arrow"]`, `is_doubled: false` |
| 二重線 | `is_doubled: true` |
| 削除 | annot を消す |

### 4.2 グローバル状態(`src/Global/initializeGlobalState.ts`)

追加:

- `line_edit_state: null | { annot_index: number, ... 編集中 UI 用フィールド }`
  - 編集 window が active な線。既存の `editing_line_label` を一般化したもの。既存実装を置き換える形で導入してよい
  - 「離脱イベントで commit」ロジックはこの state の null 化に集約する

維持:

- `line_type: TLineType` は toolbar 由来の既存路線として残置。新動線では編集 window の chip に書く

## 5. 既存コードへの影響(関連ファイル)

- スキーマ: [`src/Global/TAnnotation.ts`](../../work/kozaneba/src/Global/TAnnotation.ts) — 変更不要
- 線の描画(現実装): [`src/Canvas/Annotation/LineAnnot.tsx`](../../work/kozaneba/src/Canvas/Annotation/LineAnnot.tsx)
- レイヤ: [`src/Canvas/Annotation/AnnotationLayer.tsx`](../../work/kozaneba/src/Canvas/Annotation/AnnotationLayer.tsx) — root の `pointerEvents: "none"` 維持
- グローバル状態: [`src/Global/initializeGlobalState.ts`](../../work/kozaneba/src/Global/initializeGlobalState.ts)
- 既存 line mode toolbar: [`src/Toolbar/`](../../work/kozaneba/src/Toolbar/) 配下 — 残置
- 既存 mouse handler: [`src/Event/onCanvasMouseDown.tsx`](../../work/kozaneba/src/Event/onCanvasMouseDown.tsx) など
- 既存 `9e7122b` の inline label editing(double-click 起点)は本再設計に置き換え

## 6. 自動テストの制約 / 報告必須項目

### 6.1 禁止事項

- **`{ force: true }` 系の hit testing バイパスを使わない**
  - Cypress: `cy.click({ force: true })` / `cy.dblclick({ force: true })` / `cy.trigger({ force: true })` 全て禁止
  - Playwright: `locator.click({ force: true })` 等も同じく禁止
- 理由: 前回 PR `9e7122b` は `dblclick({ force: true })` でテストが pass、実ブラウザでは pointer events が届かない、という最悪状態を作った
- どうしても `force: true` を使わざるを得ない箇所が出た場合は **その箇所の存在と理由を PR description に報告必須**

### 6.2 推奨事項

- **実 DOM の pointer event が target に届くことをテストする**
  - hover → anchor 表示 → anchor から mousedown → mousemove → mouseup の流れを Cypress で実行
  - 編集 window 内の caret に keypress、chip に click、handle drag、Esc / Enter、別 click による離脱 commit
- 既存 unit テストとの整合性を保つ(`npm test`)

### 6.3 報告必須項目(PR description に書く)

- 採用した hover anchor の視覚(4 方向小点 / ring / `+` / 他)と、その判定軸
- 「中点 affordance」の hit area(default 表示と hover 拡張のピクセル数)
- 離脱 = commit ルールが満たすイベントの完全リスト(spec の §2.3 と差分があれば明示)
- `{ force: true }` を使った箇所(あれば、無ければ「無し」と明記)
- 人間検証(後述 §7)で気づいた違和感(あれば)

## 7. 完了条件(順序付き、上から)

**この順序を守る**。上の項目が満たされていないのに下を進めた状態で「完了」を宣言しない。

1. **人間が dev server で実ブラウザを起動し、新動線を実際に使う**
   - 起動方法は CLAUDE.md の方針に従い `npm run dev` 等で localhost を立てる
   - PR description に「人間検証用の手順」を明記(`npm run dev` → `http://localhost:3000` → kozane を 2 つ並べる → hover anchor → drag → release → 編集 window でラベル「test」 → Esc で commit → passive 線の label text を click → 編集 window 復帰、…)
2. **思考フローを止めずに使えることを確認**
   - dialog が立ち上がらない、線種選択で迷わない、誤って線を作っても 🗑 で即消せる
   - hover anchor が「default で見えすぎない」(常時 visible で kozane の見た目を壊さない)
3. 既存 Jest / Cypress / build が壊れていない(`npm test` / `npm run build` / `npm run codex:preflight`)
4. 新規追加テストは hit testing バイパス(`force: true`)を使っていない
5. PR description に以下を記述:
   - どの hard constraint(§0)を意識して、どの設計選択をしたか
   - §6.3 の報告必須項目

**自動テストが pass しただけでは(3)(4) しか満たしていない**。(1)(2) は人間が実機で確認する段階。Codex 環境では実ブラウザを完全に再現できないので、(1)(2) は「人間検証用の手順を書く」までを Codex の責務とし、実行は spec writer / nishio 側で行う。

## 8. PR テンプレート

タイトル: `[codex] Redesign line creation UI: hover anchor + edit window + leave-commit`

```markdown
## Summary
- (実装した動線の 2-3 行サマリ)

## Hard constraints honored
- (§0 のうち本 PR がどう守ったか、項目ごとに 1 行)

## Out of scope (deferred)
- (§1 の out-of-scope のうち、本 PR で意識して触らなかった点)

## Design choices (with rationale)
- hover anchor 視覚: 〜 / 判定軸: 〜
- 中点 affordance hit area: 〜 / 判定軸: 〜
- 離脱 = commit イベントリスト: spec §2.3 と差分(あれば)

## Verification
- `npm test`
- `npm run build`
- `npm run codex:preflight`
- Cypress: (追加テスト名と、`force: true` を使わずに pointer event が届くことをテストした旨)
- **人間検証用手順**(spec writer / nishio が実行):
  1. `npm run dev`
  2. `http://localhost:3000` で空の Ba に kozane を 2 つ並べる
  3. kozane に hover → anchor が縁に薄く表示されることを目視
  4. anchor から別 kozane へ drag → release → 編集 window が出ることを確認
  5. caret に「test」と入力 → Esc → label 付き線が確定
  6. ...(続く、各ステップは 1-2 行で書く)

## Notes
- `{ force: true }` を使った箇所: 無し / 〜
- 仕様と衝突した hard constraint: 無し / 〜(あれば、勝手に破らず PR で問題提起)
```

## 9. 関連 wiki / コード(参照)

- [線UI 再設計 2026-06](線UI再設計_2026-06.md) — 設計案の本体(本 prompt の元)
- [AI 委託の設計と検証 2026-06](AI委託の設計と検証_2026-06.md) — spec template の元
- [Plan B 試行 2026-06](Plan_B試行_2026-06.md) — 前回試行(辺ラベル単独)の人間検証結果
- [辺ラベル](../concepts/辺ラベル.md) — 設計史(2026-06-03 訂正含む)
- [線を引く機能](../concepts/線を引く機能.md) — 設計史(drag 不変条件 / default で線が増えない原則)
- [Kozaneba コード構造調査 2026-05](../sources/kozaneba-code-architecture.md) — `AnnotationLayer.tsx` の `pointerEvents: "none"` 設計の根拠
- [Release Notes / フォーラム 2021-2025 要約](../sources/release-notes-2021-2025.md) — 2022-2023 の線関連 UX 改修と 2021-09 `949a1fd: all lines are now non-clickable`

## Sources

- [線UI 再設計 2026-06](線UI再設計_2026-06.md)
- [AI 委託の設計と検証 2026-06](AI委託の設計と検証_2026-06.md)
- [Plan B 試行 2026-06](Plan_B試行_2026-06.md)
- [線を引く機能](../concepts/線を引く機能.md)
- [辺ラベル](../concepts/辺ラベル.md)
- [Kozaneba コード構造調査 2026-05](../sources/kozaneba-code-architecture.md)
