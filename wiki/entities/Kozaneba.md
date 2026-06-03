---
title: Kozaneba
type: entity
created: 2026-05-16
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md
  - raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md
  - raw/kozaneba-forum/
  - raw/kozaneba-forum-jp/
  - work/kozaneba
---

## 定義

**Kozaneba** = 「かんがえをまとめるデジタル文房具」。Web アプリ。OSS、無償提供。作者: nishio。

- 公式チュートリアル / 試用: https://kozaneba.netlify.app/
- ステータス: 実験的実装フェーズ(安定性重視ではない)

## モチベーション

KJ法的な手法は有益だが、紙でやると不便がある。よいデジタル文房具が欲しい。nishio 自身がまず使いたいツールとして始まったが、現在は「**他の人に使われて成長すること**」を主目的に据えている。詳しくは [Kozanebaを作ることで何がどうなればいいのか](../themes/なぜ作るのか.md)(未作成)。

## 主要機能

- **グループを畳む** — 大きくなったまとまりを一時的に折りたたんで全体を見渡しやすくする
- **重要なものを大きくする** — サイズで重要度を表現
- **ものの間に関係の線を引く** — ノード間のつながりを明示

## 系譜

- 前身: [Regroup](Regroup.md) → [Movidea](Movidea.md)
- 関連:
  - [Keichobot](Keichobot.md) — 言語化を促す対話ボット。Kozaneba と対比して論じられることが多い(「[Keichobot は言語化し Kozaneba は一次元化する](../../raw/scrapbox_kozaneba/2021-12-24__Keichobotは言語化しKozanebaは一次元化する.md)」、テーマページ [Kozaneba vs Keichobot](../themes/Kozaneba_vs_Keichobot.md))
  - [Scrapbox](Scrapbox.md) — リンクベースのノートツール。Kozaneba とは構造的に対比される(「[ScrapboxとKozanebaの構造の比較](../../raw/scrapbox_kozaneba/2023-11-04__ScrapboxとKozanebaの構造の比較.md)」、テーマページ [Kozaneba vs Scrapbox](../themes/Kozaneba_vs_Scrapbox.md))

## 2026-05-16: 現状(Plan B)と次世代(Plan A)の二段構え

[3 Plan 議論](../themes/3plan議論.md) で、Kozaneba の今後について **両方を並行する** が選ばれた:

- **現 Kozaneba(Plan B 改造の対象)**: nishio 自身の 300 件作業の加速、[源の長文](../concepts/源の長文.md) データモデル拡張、現 React/DOM 実装の継続。Canvas 化([Canvas移行の検討](../themes/Canvas移行の検討.md))は急がない方向
- **次世代(Plan A、新規サービス)**: [Keichobot](Keichobot.md) / [いどばた](いどばた.md) / Kozaneba の 3 つを参考にした **全く新しいサービス**。データモデルから新規

Plan A は Kozaneba の直接の継承ではなく、Kozaneba を deprecate するか並列に保つかは未決。Plan B 期間中に Plan A の要求仕様を貯める方針(現 Kozaneba を使いながら「やりたかったができなかった」操作・AI と対話したかった場面・関係を引きたかったが諦めた瞬間 etc を記録)。

## 2026-05-19: 3 つのストーリーの中での位置づけ

[3つのストーリー比較](../themes/3つのストーリー比較.md) では、Kozaneba は 3 方向のうち少なくとも 2 方向の中心にいると整理された:

- **改善ストーリー**では、Kozaneba 自体が直接の改造対象。辺ラベル、Selection 操作、[源の長文](../concepts/源の長文.md) 接続を通じて、いまの作業場を強くする
- **似たものを新しく作るストーリー**では、Kozaneba は継承元。残すべき本質は「前言語的な構造化」「空間でのねりねり」「関係探索」
- **全く新しいものを作るストーリー**では、Kozaneba は直接の UI 継承元というより、洞察の供給源。入口は canvas でない可能性がある

この整理により、Kozaneba は「改善される現役プロダクト」であると同時に、「次世代設計の観察装置」でもある、と位置づけ直せる。

## 2026-05-25: 実装上の事実(コード一次調査より)

[work/kozaneba コード構造調査](../sources/kozaneba-code-architecture.md) で `work/kozaneba/`(main, 5de81c2)を読んで判明した実装上の事実:

- 技術スタック: React 18 + TypeScript + Firebase + Netlify、状態管理は `reactn` の単一グローバル state
- Item は **4 種類の Union**(`kozane / group / scrapbox / gyazo`)。「源の長文」を持つ型は無い
- Annotation は **`line` 1 種類のみ**で、`items: Array(RTItemId)`(N項可)と `label: String.optional()`(辺ラベル可)が既にデータモデル上は実装済み
- `package.json` の `name` は依然 `"movidea"`([Movidea](Movidea.md) からの一本道のコードベースを継承)
- 物理演算は全 Item ペア走査 + 線の重心ばねという素朴実装で O(N²)
- 隠しフラグ `kozaneba.constants.exp_no_adjust` で「`#` で始まる Kozane を見出し風に表示」する実験機能

これは Plan B での最小改造を考えるとき、「**辺ラベル UI**」と「**源の長文フィールド**」が独立した 2 軸の改造起点になることを示唆する([3 Plan 議論](../themes/3plan議論.md))。前者はデータモデルに手を入れず UX 動線だけで済むが、後者はデータモデルそのものの拡張が必要。

## 公開リリースのタイムライン

[Release Notes 2021-2025 要約](../sources/release-notes-2021-2025.md) で外向きにアナウンスされた機能変更を時系列で整理。重要な節目:

- **2021-08-02**: First release ([Movidea](Movidea.md) → Kozaneba 改名と同時)
- **2021-08-20**: Beta release。日英フォーラム同時開設(英 [kozaneba-forum](https://scrapbox.io/kozaneba-forum/) / 日 [kozaneba-forum-jp](https://scrapbox.io/kozaneba-forum-jp/)、両方を最初から)
- **2021-09-08**: Annotation Layer (矢印機能の最初の実装)
- **2022-03-24**: Scrapbox Integration の本格化
- **2022-05-31**: ブラウザ言語設定で日本語/英語チュートリアル自動切替(JP リリースノートにのみ記載)
- **2023-02-27**: Tear / Merge 追加・Split 削除
- **2025-04-27**: 外部ユーザ報告由来の bug fix 集中リリース (PR #18-21, #29、JP リリースノートにのみ記載)。iPad 互換性改善、チュートリアル section 2 crash 修正、白画面 Ba 修正、Gyazo 0 サイズ box 修正
- **2025-08-09**: メニュー画像が壊れていた問題の修正 (PR #32、JP のみ)。原因は Gyazo がリダイレクト HTML を返す変更
- **2025-09-11**: Merge 挙動変更・Scale Double 追加・Selection menu への変形操作集約 (PR #33-35)

EN リリースノートと JP リリースノートには **明確な差**がある: 2022-05-31 / 2025-04-27 / 2025-08-09 のエントリは **日本語版にのみ**存在する。EN 側の更新は 2025-09-11 で 2023-02-28 から直接ジャンプしている。

## 外部ユーザの活動

公開フォーラムには複数の外部ユーザが要望・バグ報告・質問を投稿している:

- `Foam_Crab` — 2021-08-31「Scrapboxのページをグルーピングしたい」要望(同日リリース)
- `uchan_nos` — 2022-01-21「ホイールのドラッグでBaを移動したい」(CAD 風)、2022-01-25 中ボタンドラッグ実装
- `kusanagi` / `k937gy` — 2022-01-25 Typo 報告 / ホイール感度フィードバック
- `sta` (@sta) — 2022-08-10「Add Lines の挙動を知りたい」混乱報告。nishio 応答: 「線を引く機能は今後改善したいと思っていますが、**大手術になる**と思います」。2023-02-06 の中点ベース左右判定はこの混乱への部分的応答
- `YJ` — 2022-09-21 存在しない Scrapbox page の crash bug 報告、2023-01-16 fix
- `reira` — 2023-11-07 チュートリアル section 2 で crash 報告、2025-04-27 fix (PR #18)
- `hoshihara` — 2024-02-27 白画面 Ba bug 報告、2025-04-27 fix (PR #21)

これは「**他の人に使われて成長すること**」(2021-08-08 [Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)) が部分的に実現していたことを示すが、活発な発展期(2022-2023)と長期メンテモード(2024-2025 春)の境目もはっきり見える。

## このリポジトリでの分量

`raw/scrapbox_kozaneba/` に 403 ページ(タイトルに Kozaneba を含むものが 117 ページ)。最初のメンション 2017-09-03、最新 2026-05-16。年別の分布:

| 年 | ページ数 |
|---|---|
| 2017 | 1 |
| 2018 | 1 |
| 2019 | 1 |
| 2020 | 2 |
| 2021 | 102 |
| 2022 | 86 |
| 2023 | 131 |
| 2024 | 35 |
| 2025 | 36 |
| 2026 | 8 |

開発・思索のピークは 2021〜2023。

## Sources

- [かんがえをまとめるデジタル文房具Kozaneba](../../raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md)
- [Kozaneba](../../raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md)
- [raw/kozaneba-forum/](../../raw/kozaneba-forum/) — 公式英語 forum 全 17 ページ(Release Notes 含む、2021-06 〜 2025-09)
- [raw/kozaneba-forum-jp/](../../raw/kozaneba-forum-jp/) — 公式日本語 forum 全 48 ページ(リリースノート、ユーザ要望・バグ報告、チュートリアル和訳、2021-08 〜 2025-08)
