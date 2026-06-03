---
title: Kozaneba コード構造調査(2026-05)
type: source
created: 2026-05-25
updated: 2026-05-25
sources:
  - work/kozaneba
---

`work/kozaneba/`(main, 最終コミット 5de81c2 = PR #36 マージ)を一次ソースとして読んだコード構造の要約。Scrapbox メモが「設計意図」「議論」を残すのに対し、本ページは **2026-05 時点で実装されている事実** を記録する。Scrapbox 記述と現コードに齟齬がある場合の判定基準として参照する。

## 技術スタック

- **フロント**: React 18 + TypeScript、状態管理は [reactn](https://github.com/CharlesStover/reactn)(`setGlobal` / `useGlobal` で 1 つのグローバル state を共有)
- **データ型バリデーション**: `runtypes`(`RTKozaneItem`, `RTGroupItem`, `RTScrapboxItem`, `RTGyazoItem`, `RTLineAnnot`)
- **永続化**: Firebase Auth + Firestore + Cloud Functions(Scrapbox API プロキシ、汎用 CORS プロキシ)
- **ホスティング**: Netlify
- **テスト**: Jest + Cypress E2E
- **コードネーム**: `package.json` の `name` は依然として `"movidea"` のまま([Movidea](../entities/Movidea.md) 時代の遺物が公式リポジトリにも残存)

## グローバル状態スキーマ

[`src/Global/initializeGlobalState.ts`](../../work/kozaneba/src/Global/initializeGlobalState.ts) で `INITIAL_GLOBAL_STATE` として定義。主要フィールド:

- `drawOrder: TItemId[]` — 描画順(=「場」上の Item の z 順 + 列挙)
- `itemStore: { [id: string]: TItem }` — Item 本体
- `annotations: TAnnotation[]` — 注釈(現状は line のみ)
- `selected_items`, `selectionRange`, `is_selected`, `clicked_target`, `drag_target`, `mouseState` — 選択/ドラッグ状態
- `scale`, `trans_x`, `trans_y` — キャンバスのビュー変換
- `user`, `cloud_ba`, `writers`, `anyone_writable` — Firestore 連携
- `in_tutorial`, `tutorial_page` — チュートリアル状態
- `line_start`, `line_type` — 線描画モード(`"line" | "arrow" | "double_heads" | "double_lines" | "delete"`)
- `print_mode`, `language`, `did_warn_over_capacity`

「場」(Ba)というドメイン語は state に直接の field として現れず、`itemStore + drawOrder + annotations + scale/trans + title + writers` の集合で表現される。

## Item 階層

[`src/Global/TItem.ts`](../../work/kozaneba/src/Global/TItem.ts) は **4 種類の Union**。

| type | 主なフィールド | 役割 |
|---|---|---|
| `kozane` | `text`, `position`, `scale`, `custom.{ style?, url? }` | 通常のこざね。`custom.url` でリンク化 |
| `group` | `text`, `position`, `items: TItemId[]`, `isOpen`, `scale`, `custom.style?` | グループ。畳まれた状態(`isOpen: false`)では Nameplate Kozane として表示 |
| `scrapbox` | `text`, `image`, `url`, `description: string[]`, `position`, `scale` | Scrapbox ページ参照([Scrapboxこざね](../concepts/Scrapboxこざね.md)) |
| `gyazo` | `url`, `text`, `position`, `scale` | Gyazo 画像 |

つまり Kozaneba の「Item」は **テキスト断片(Kozane)+ グループ + 外部参照 2 種(Scrapbox / Gyazo)** で構成される。**こざね自体に「源の長文」を保持するフィールドは無い**([源の長文](../concepts/源の長文.md))。Scrapbox 型のみが間接的に「source への参照」を URL として持つ。

旧名 `piece` は依然としてマイグレーションコードに残っており、`docdate_to_state` で `to_item()` が `type === "piece"` を `kozane` として通している([Kozane.tsx の祖先名 `piece`](../../work/kozaneba/src/Cloud/FirestoreIO.ts))。

## Annotation スキーマ(線/矢印)

[`src/Global/TAnnotation.ts`](../../work/kozaneba/src/Global/TAnnotation.ts) は **`line` 1 種類だけ**。重要な発見:

```ts
RTLineAnnot = Record({
  type: Literal("line"),
  items: Array(RTItemId),          // ← N 個。データレベルでは N項関係
  heads: Array(RTArrowHead),        // "none" | "arrow", items と並列
  is_doubled: Boolean,              // 二重線フラグ
  label: String.optional(),         // ← 辺ラベルのフィールドは既に存在
  custom: Record({
    arrow_head_size, is_clickable, stroke_width, opacity
  }),
});
```

ここから読める含意:

1. **辺ラベルのデータモデルは 2025-09 の Devin 実装で既に入っている**([辺ラベル](../concepts/辺ラベル.md))。残る課題はラベル入力 UI のみ。
2. **N項関係はスキーマ上既に実装されている**([N項関係](../concepts/N項関係.md))。`items: Array(RTItemId)` であり、長さに制限はない。`heads` も同じ長さの並列配列なので「各端点ごとに矢印頭の有無」を持てる。
3. **線種(`line | arrow | double_heads | double_lines | delete`)は state の `line_type` だけで、annotation 自体は `heads[]` と `is_doubled` の組合せで全パターンを表現**。`delete` は描画モード(描いた線で交差した既存線を削除)で、データには残らない。

## なぜ「エッジにイベントリスナがついてない」のか

2025-09-14 の Scrapbox メモ「[Kozanebaにエッジラベルをつけると良いのでは](../../raw/scrapbox_kozaneba/2025-09-14__Kozanebaにエッジラベルをつけると良いのでは.md)」で「エッジにイベントリスナがついてない」と書かれていた件、コードで確認:

[`src/Canvas/Annotation/AnnotationLayer.tsx`](../../work/kozaneba/src/Canvas/Annotation/AnnotationLayer.tsx):

```tsx
<svg style={{ ..., pointerEvents: "none" /* paththrogh event on background */ }} >
```

→ SVG レイヤ全体で `pointerEvents: "none"` を指定し、背景キャンバスにイベントを通している。
[`src/Canvas/Annotation/Line.tsx`](../../work/kozaneba/src/Canvas/Annotation/Line.tsx) で個別の `<line>` に対しては `is_clickable` フラグでのみ `pointerEvents: "auto"` を復活させる仕組み。**既存の線はデフォルトで `is_clickable: undefined` = false なので、クリックできない**。

つまりエッジを操作対象にするには既存全 line annotation の `custom.is_clickable` を true に切り替える必要がある(あるいはデフォルトを変える)。これは設計判断としてあえて切ってある余地もある(ドラッグ中のヒット判定を線に取られると邪魔)。

## 物理演算の実装

[`src/Physics/`](../../work/kozaneba/src/Physics/) に集約。3 つの法則を加算合成する設計([`physics.ts`](../../work/kozaneba/src/Physics/physics.ts) の `PhysicalLaw = (state: State) => Gradient`):

| 法則 | 役割 |
|---|---|
| `ItemRepulse` | 全 Item ペア間距離 `n < RADIUS = √(KOZANE_WIDTH² + KOZANE_HEIGHT²)` のとき反発。力は `(RADIUS - n)/2` を法線方向に |
| `LineSpring` | 各 line annotation に対し、参加項の重心点 `gp` に向けて、自然長 `NL = KOZANE_WIDTH` を超える項を引き寄せる |
| `pin` | ユーザがピン留めした Item を固定 |

更新は `AdaDelta` か `GradientDecent`(両方クラス定義あり)で、勾配を消化する。終端速度 `TERMINAL_VELOCITY = 200` で発散を抑える。**全 Item を毎ステップ走査する O(N²)** なので、大規模化での足を引っ張る要因がここ([Canvas移行の検討](../themes/Canvas移行の検討.md))。

2021-09 開発日記での「ステップ自体は 200ms くらい」([物理演算](../concepts/物理演算.md))に対応する実装。Scrapbox メモが残した「描画が重い」「メインの目的ではない」という葛藤と、コード上のシンプルなペア走査が裏付け合う。

## URL ルーティング(hash ベース)

[`src/App/App.tsx`](../../work/kozaneba/src/App/App.tsx) は hash を見て分岐:

- `#` なし → `TopPage`(チュートリアル)
- `#blank` → 空の場
- `#tinysample` → サンプル
- `#new` → 新規 Ba 作成
- `#edit=<ba>` → 既存 Ba の編集
- `#view=<ba>` → 閲覧モード

サーバ側ルーティングを持たず、全部クライアントで完結。Firestore の `ba` コレクションが場の集合。

## window.kozaneba API と UserScript

[`src/API/KozanebaAPI.ts`](../../work/kozaneba/src/API/KozanebaAPI.ts) で `window.kozaneba` を公開し、ブラウザコンソール / [UserScript](../concepts/UserScript.md) から add_kozane / add_arrow / show_dialog / start_tutorial / physics step / Scrapbox 取込み / JSON import 等を叩ける。これにより「ユーザがメニューを自作する」設計が可能。

## Selection 操作と最近の追加(2025-09)

[`src/Selection/`](../../work/kozaneba/src/Selection/) と [`src/Menu/SelectionMenu.tsx`](../../work/kozaneba/src/Menu/SelectionMenu.tsx) で複数選択時のメニュー。2025-09 の PR #34 / #35 で `Rotate`, `Spread`, `Scale Double` が選択メニューに統合された。`Selection の中心を pivot にして変換` が修正点([git-history 要約](kozaneba-git-history-2025.md))。

## merge セマンティクス変更(2025-09)

`Group menu` の `Scale Double` 追加と並んで、merge を「新しいこざねを作る」から「**大きいこざねが残る**」へ変更。これは「自分の手元の構造を破壊しない」原則と整合する。

## 隠しフラグ: `exp_no_adjust`

[`src/Kozane/Kozane.tsx`](../../work/kozaneba/src/Kozane/Kozane.tsx#L38-L48) に実験フラグ `kozaneba.constants.exp_no_adjust`。true かつ Kozane テキストが `#` で始まる場合、フォントサイズ調整を切り、`#` を取り除いた本文を「見出し風の幅自由テキスト」として表示する。Scrapbox メモには登場せず、現役の隠し機能。

## Firestore 権限モデル

[`firestore.rules`](../../work/kozaneba/firestore.rules) は:

- `create: if true`(誰でも場を作れる)
- `update/delete: anyone_writable || writers に自分が含まれる`
- `get: if true`(誰でも読める)
- `list: if is_writer()`

`anyone_writable: true` がデフォルトで、「URL を知っていれば誰でも編集可」が前提。共有モデルは Scrapbox の `everyone is editor` パターンに近い。

## このコード読解からの含意(他ページへの fill back)

1. **辺ラベル UI** は新規データモデル拡張ではなく、**既存スキーマの label フィールドへの入力経路と表示・編集 UI** の問題に絞れる([辺ラベル](../concepts/辺ラベル.md))
   - **2026-06-03 訂正**: 「入力 UI 問題に絞れる」は楽観的だった。[Plan B 試行 2026-06](../themes/Plan_B試行_2026-06.md) で Codex 実装 `9e7122b` が SVG `pointerEvents` の親子上書き挙動で動かないことが判明、かつ仮に直しても本コード調査 §「なぜ『エッジにイベントリスナがついてない』のか」で記録された **「線を click 可能にすると kozane drag が妨害される」設計原則** を破る。問題は「label フィールドへの入力経路」ではなく **線生成 UI 全体の commit 瞬間の欠落** にあった。統一再設計は [線UI 再設計 2026-06](../themes/線UI再設計_2026-06.md)
2. **N項関係** は「`items[]` を 3 以上にする UI 動線」の問題であり、データモデル拡張は不要([N項関係](../concepts/N項関係.md))
3. **物理演算** は O(N²) のペア走査が本質で、大規模化と相性が悪い。Canvas 化と無関係に物理側のスケーリングも考える必要がある([物理演算](../concepts/物理演算.md))
4. **源の長文** はやはり現状の Item 4 種のどこにも入っていない。データモデルに新フィールドを足すか、新しい Item type を入れるか、別 collection に分けるかの判断が必要([源の長文](../concepts/源の長文.md))
5. **Plan B 改造** の初手は「(a) annotation に既存 `label` フィールドの入力 UI を載せる」「(b) Kozane に `source_situation` フィールドを足す」が独立した 2 軸の最小改造で、それぞれ Plan A 設計の検証材料になる([3 Plan 議論](../themes/3plan議論.md))

## Sources

- [`work/kozaneba/`](../../work/kozaneba/) — main, 5de81c2 時点のソース
- [Kozaneba git history 2025 要約](kozaneba-git-history-2025.md) — 同じリポジトリの git log 視点
