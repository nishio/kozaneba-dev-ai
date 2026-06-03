---
title: Release Notes / フォーラム 2021-2025 要約
type: source
created: 2026-06-03
updated: 2026-06-03
sources:
  - raw/kozaneba-forum/
  - raw/kozaneba-forum-jp/
---

# Release Notes / フォーラム 2021-2025 要約

[Kozaneba](../entities/Kozaneba.md) 公式の英語フォーラム ([kozaneba-forum](https://scrapbox.io/kozaneba-forum/)、17 ページ) と日本語フォーラム ([kozaneba-forum-jp](https://scrapbox.io/kozaneba-forum-jp/)、48 ページ) を 2026-06-03 に Cosense CLI で全件取得して整理した要約。両方のフォーラムは [2021-08-20 のベータリリース時点で同時に開設されている](../themes/設計判断ログ.md)。

姉妹ページとの関係:

- [Kozaneba git history 2025 要約](kozaneba-git-history-2025.md) — 2025 年のコード変更を git log から読んだ要約。2025-04-27 / 09-11 のリリースはこれと突き合わせ可能
- [設計判断ログ 2021](../themes/設計判断ログ.md) — 2021 年の Scrapbox 開発日記。フォーラム側のリリースノートは公開告知の側面が強く、開発日記とは粒度が異なる
- [Kozaneba コード構造調査 2026-05](kozaneba-code-architecture.md) — 現コードの状態

## 英語版と日本語版リリースノートの差分

EN と JP の Release Notes / リリースノート ページは内容がほぼ並行しているが、**JP にのみ存在するエントリ**がある:

- **2025-08-09**: メニューの画像が壊れていたのを直した (PR #32)。Gyazo が PNG 画像の代わりに 135 byte の HTML リダイレクトを返すようになっていたのが原因 (12 PNG 全てが破損)
- **2025-04-27**: 外部ユーザ報告由来の bug fix 集中リリース (PR #18-21, #29)
  - PR #18: チュートリアル section 2 を選択するとクラッシュする問題を修正 (報告者: reira)
  - PR #19: メニューの Gyazo 画像を静的アセットに置き換え
  - PR #20: iPad での互換性を改善 (2021-08-21 [タッチデバイスで使えないバグ] からの宿題)
  - PR #21: 画面が真っ白で何も表示されなくなった Ba の修正 (報告者: hoshihara)
  - PR #29: Gyazo 画像の 0 サイズボックス問題の根本的な修正
- **2022-05-31**: ブラウザの言語設定が日本語である場合、チュートリアルが日本語で表示されるようになった

EN 側のリリースノートは 2025-09-11 から 2023-02-28 まで直接ジャンプしており、2025-04-27 / 08-09 の修正は **英語ユーザにはアナウンスされていない**。これは「**日本語ユーザコミュニティの方が活発な反応を引き出した**」という事実とも整合する(下記「外部ユーザ」参照)。

## 期間別の重心移動

- **2021-06 〜 2021-08**: 前身 Movidea からの再構築・改名・初公開。Routing、Range Selection、Group の開閉、Cypress 自動テスト、Firebase 認証・cloud save、Tutorial、Sentry、UserScript 拡張
- **2021-09 〜 2021-10**: 矢印 (Annotation Layer) と物理演算の最初の実装。Scrapbox / Gyazo / Link Kozane、ユーザによる Server API
- **2021-12 〜 2022-01**: 哲学書読解からの bug 修正と「Leave from lines」「中ボタンドラッグで視点移動」(外部ユーザ uchan_nos の CAD 風要望から)
- **2022-03 〜 2022-07**: Scrapbox Integration の本格化、画像 URL パターン拡充、矢印の半透明化・favicon、PDF 出力、サイズ 2倍/半分メニュー、保存容量制限の発見(2048 objects)、日本語チュートリアル自動化
- **2023-01 〜 2023-02**: 線関連の UX 改修ピーク。Context menu からの線作成、`to_adjust_opacity_of_lines_by_length` API、中点ベースの左右判定、Drag = Selection、Tear / Merge 追加と Split-Kozane 撤去、Rotate / Spread
- **2023-03 〜 2025-03**: 約 2 年の沈黙。フォーラムには外部ユーザのバグ報告のみ(reira 2023-11、hoshihara 2024-02)
- **2025-04-27**: 蓄積した外部ユーザ報告をまとめて修正(JP RN のみアナウンス)
- **2025-08-09**: Gyazo 仕様変更由来の menu 画像破損を修正(JP RN のみ)
- **2025-09-11**: Merge 挙動の意味論変更、Group menu に Scale Double、Selection Menu に Rotate / Spread / Scale Double 集約 (PR #33-35、EN+JP 両方)

## 機能カテゴリ別の主な変遷

### 線・矢印 ([線を引く機能](../concepts/線を引く機能.md))

| 日付 | 変更 |
|---|---|
| 2021-09-08 | Annotation Layer 追加 (矢印のみ、GUI なし、JSON import のみ) |
| 2021-09-09 | Selection menu から矢印追加 |
| 2021-09-16 | 矢印追加 API |
| 2021-09-17 | 両頭矢印 menu。`right is head` 明示 |
| 2021-09-28 | 矢印追加 menu を画像化 |
| 2021-10-12 | 二重線/矢印・矢頭なし線(「related but not important」「this and that are same」を近接以外で表現) |
| 2022-01-25 | 「Leave from lines」menu(線連想からの離脱) |
| 2022-03-08 | フォーラムに [Drawing lines](../../raw/kozaneba-forum/Drawing_lines.md) (EN) / [線を引く機能](../../raw/kozaneba-forum-jp/線を引く機能.md) (JP) 公開 — 公式の使い方ガイド |
| 2022-05-26 | 矢印 head と body を一緒に半透明化、長さに応じて opacity が滑らかに変化、分岐・二重線も同様 |
| 2022-08-10 | 外部ユーザ sta が [Add Lines の挙動を知りたい](../../raw/kozaneba-forum-jp/解決:Add_Linesの挙動を知りたい.md) を投稿。nishio 応答「線を引く機能は今後改善したいと思っていますが、**大手術になる**と思います」 |
| 2023-01-26 | Context menu からの線作成 UX、`Enter` で AddKozane、`to_adjust_opacity_of_lines_by_length` 公開 |
| 2023-02-06 | drag = selection、中点ベースの左右判定 (sta の混乱への部分応答) |
| 2023-02-27 | Tear / Merge 追加、Split-Kozane 撤去 |
| 2025-09-11 | Merge の意味論変更 (新規生成 → 最大 scale の kozane が生存) PR #33 |

### グループ

| 日付 | 変更 |
|---|---|
| 2021-07-13〜09-02 | Title 表示、ドラッグ移動、Group 開閉、Title 編集、context menu 化、padding 調整、ungroup 時 title を空 group として残す(「title はグループの要約として残すべき」思想) |
| 2021-10-12 | close 時のサイズを最大子要素サイズに |
| 2022-07-06 | Group title font 拡大、x2/half サイズ menu |
| 2023-02-28 | Rotate (反時計回り)、Spread (間隔を 2 倍に展開) |
| 2025-09-11 | Scale Double (PR #34、間隔+サイズ両方 2 倍)、Rotate/Spread/Scale Double を Selection Menu でも (PR #35) |

### Scrapbox / Gyazo / 外部リンク ([Scrapboxこざね](../concepts/Scrapboxこざね.md))

| 日付 | 変更 |
|---|---|
| 2021-08-30 | Scrapbox / Gyazo Kozane 追加、expand menu で 2hop 全 page kozane 化 |
| 2022-03-24 | Scrapbox Integration: project 名を設定すれば kozane を Scrapbox 記法でパース、アイコンを画像化 |
| 2022-05-26 | フォーラムに [Scrapbox Integration](../../raw/kozaneba-forum/Scrapbox_Integration.md) 解説公開。リンクをクリック可能にしない理由を明文化(「こざねを動かすことが最頻出、リンクで誤動作する」) |
| 2022-05-30 | favicon を外部リンク kozane に付与 |
| 2022-06-03 | Image URL パターン拡張、`*.png`、`[]` 囲み、Scrapbox からそのまま貼り付け可能 |
| 2022-06-09 | Scrapbox Integration ON 時、context menu に「expand scrapbox links」 |
| 2022-09-21 | 外部ユーザ YJ が[存在しない Scrapbox page で crash bug](../../raw/kozaneba-forum-jp/解決:存在しないScrapboxページのこざねを作るとクラッシュ.md) を報告 |
| 2023-01-16 | YJ 報告の crash を修正、3 種状態 (linked-empty / not-found / project-not-found) を明示 |

### 物理演算 ([物理演算](../concepts/物理演算.md))

| 日付 | 変更 |
|---|---|
| 2021-09-08 | Automatic shaping by physics engine (ON/OFF は API のみ) |
| 2021-09-16 | Physics vibration を低減、`unpin` を System menu → User custom menu へ退避 |

`unpin` を user custom 側に退避して以降、リリースノートに **一切登場しない**。2025-06 時点でも default 機能化されておらず、nishio 本人は「現時点では重視していない」と確認済(2026-06 セッション)。「機械が勝手に動かしてはいけない」禁忌の継続。

### UserScript / API 拡張 ([UserScript](../concepts/UserScript.md))

| 日付 | 変更 |
|---|---|
| 2021-08-20 | `Modify Constants` 有効化 |
| 2021-08-23 | UserScript 機構 (load 時実行、Tutorial 上書き、AppBar ボタン追加) |
| 2021-08-26 | Group padding を constants で調整可能に |
| 2021-08-31 | menu を UserScript で拡張可能に |
| 2021-09-01 | Selection を copy(JSON)/paste、テンプレ作成 API として公開 |
| 2021-09-09 | UserScript ダイアログ |
| 2021-09-16 | 矢印追加の bulk API、Ba を JSON で作る server API |
| 2023-01-26 | `to_adjust_opacity_of_lines_by_length` 公開 |

### Cloud / 保存 / 共有

| 日付 | 変更 |
|---|---|
| 2021-08-03〜10 | Google 連携、Cloud save、Sentry |
| 2021-08-27 | Ba Dialog (title 変更、リードオンリー共有 ACL) |
| 2021-08-28 | Ba copy |
| 2021-09-02〜03 | Delete Ba、Link Kozane (URL 持ち kozane)、共有時の権限バグ修正、IndexedDB 自動バックアップ |
| 2022-06-16 | Print to PDF (Chrome 印刷時 font-size 80% に縮小して line height bug 回避) |
| 2022-07-05 | 2048 objects 制限の発見、1000 で info / 2000 で warning |
| 2025-04-27 | 白画面 Ba bug 修正 (PR #21、hoshihara 報告) |

### Tutorial / オンボーディング ([チュートリアル](../concepts/チュートリアル.md))

| 日付 | 変更 |
|---|---|
| 2021-08-05〜10 | Tutorial 機能、~16 page まで拡充 |
| 2021-08-06 | スマホ用「It's small」メッセージ |
| 2021-08-19 | 完了後にハテナアイコンから help 再表示 |
| 2021-08-20 | フォーラムに [Tutorial Contents](../../raw/kozaneba-forum/Tutorial_Contents.md) 全文公開(machine translation 用に) |
| 2021-08-20〜2022-05-31 | JP フォーラム上で [チュートリアル和訳(2021/8)](../../raw/kozaneba-forum-jp/チュートリアル和訳(2021_8).md) → 改訂版 [チュートリアル和訳](../../raw/kozaneba-forum-jp/チュートリアル和訳.md) の手動和訳が育つ |
| 2022-05-26 | 右上に help question mark button |
| 2022-05-31 | **ブラウザ言語自動検出で日本語チュートリアル**を表示 (JP RN にのみ記載) |
| 2025-04-27 | チュートリアル section 2 crash 修正 (PR #18、reira 報告) |

「チュートリアルの英文を手動和訳 (2021-08) → 改訂 (2022-05) → ブラウザ自動切替対応 (2022-05-31)」という多言語化パスは、設計判断ログ 2021-08-10 の「**漢字 (中国語) こそ本質的に重要かも**」という宿題を **日本語に限り** 解いた形。中国語化は手付かず。

## 公開された設計判断・原則 (フォーラム解説ページから)

### 1. 「default で線が増えない方を選ぶ」原則 ([Remove Split-Kozane feature](../../raw/kozaneba-forum/Remove_Split-Kozane_feature.md))

Split 削除時 (2023-02-27) に明文化された:

> If you have a choice between a default specification with more lines and a default specification with no lines, you can say that you choose the one with no lines. I think it is better not to increase the number of lines because people get confused when there are too many lines.
>
> デフォルトで線が増える仕様と、増えない仕様とを選べる場合、増えない方を選んでいると言えます。人間は線が多すぎると混乱するので、増えない方が良いと思っています。

これは [活用されなかった機能](../themes/活用されなかった機能.md) で抽出した「線・畳む・辺ラベルの一般化が実用に負ける」パターンへの **後ろ向きの自己定理化**。

### 2. 「線をクリック可能にすると『こざねを動かす』が阻害される」 ([Scrapbox Integration](../../raw/kozaneba-forum/Scrapbox_Integration.md))

2022-05-26 に明示:

> I avoid making links as they are because I think that if I make them as they are, there will be accidents when people try to drag the kozane and open the link. [...] I once tried to make the setting menu appear by putting a hit detection on arrow annotations, but this also interfered with "moving the kozane", so I stopped!

これは [線を引く機能](../concepts/線を引く機能.md) で書いた「**エッジにイベントリスナが付いていない**」コード上の事実の **設計意図**。`AnnotationLayer.tsx` の `pointerEvents: "none"` はこの判断の継承。

### 3. 「線は本質ではない、迷うなら近接で」 ([Drawing lines](../../raw/kozaneba-forum/Drawing_lines.md))

2022-03-08 に公開:

> I have been doing the Kozane method/KJ method on paper for 10 years and has seen the benefits. In other words, drawing lines is not essential for the benefits. If you are wondering which is better between "putting them close together" and "drawing a line between them", you should better to put them close together.

公式の「**線は補助、近接が本体**」という外向きの立場。同時に「`distant objects` は必ずしも 2 つだけとは限らない。良い言葉が見つからないので『線』と呼んでいる」と [N項関係](../concepts/N項関係.md) を public に明示している。

## 外部ユーザ

フォーラムに痕跡を残した外部ユーザ:

| ユーザ | 時期 | 投稿 |
|---|---|---|
| `Foam_Crab` | 2021-08-31 | 「Scrapboxのページをグルーピングしたい」要望(当日リリース) |
| `uchan_nos` | 2022-01-21 | 「ホイールのドラッグで Ba 移動」(CAD 風)、4 日後実装 |
| `k937gy` | 2022-01-25 | Typo 報告 |
| `kusanagi` | (不明) | ホイール感度フィードバック → UserScript ガイド作成 |
| `sta` | 2022-08-10〜12 | Add Lines 挙動への混乱、nishio「大手術になる」と返答、2023-02-06 部分対応 |
| `YJ` | 2022-09-21 | 存在しない Scrapbox page で crash、2023-01-16 fix |
| `reira` | 2023-11-07 | チュートリアル section 2 crash、2025-04-27 fix (PR #18) |
| `hoshihara` | 2024-02-27 | 白画面 Ba bug、2025-04-27 fix (PR #21) |

これは [なぜ作るのか](../themes/なぜ作るのか.md) 第 2 期(2021-08-08 「他の人に使われて成長すること」)が **部分的に実現していた**ことの証拠。一方、(1) 投稿者の多くは技術系の知人圏、(2) 2024-2025 春の長期メンテモード中も外部報告は届いていたが反映までに 1-3 年かかった、という限界もある。

## 開いた問い

- **辺ラベル機能はフォーラムに 1 度も告知されていない** — `4c9b38d` / `5de81c2` で 2025-09 にコード merge されたものの、Release Notes / リリースノート 両方に記載なし。これは [辺ラベル](../concepts/辺ラベル.md) の「入力 UX が確定していない」状態と整合し、2026-06 時点でも nishio 本人が「動線がよくわからん」と確認している
- **EN/JP の更新頻度差** — JP のみに記載された 2022-05-31 / 2025-04-27 / 2025-08-09 を見ると、英語ユーザへの告知が後手に回っている。これは「**他の人に使われて成長する**」の対象が事実上日本語圏に絞られている可能性を示唆
- **中国語フォーラム/チュートリアル**は依然未着手。「漢字こそ本質的」(2021-08-10) の宿題は 5 年間未消化
- **2023-03〜2025-03 の沈黙期**にユーザは離れたか残ったか — フォーラムでは reira (2023-11)・hoshihara (2024-02) しか投稿がなく、新規流入は停止していた可能性が高い

## Sources

- [raw/kozaneba-forum/](../../raw/kozaneba-forum/) — 英語フォーラム全 17 ページ
- [raw/kozaneba-forum-jp/](../../raw/kozaneba-forum-jp/) — 日本語フォーラム全 48 ページ
- 元 URL: https://scrapbox.io/kozaneba-forum/ / https://scrapbox.io/kozaneba-forum-jp/
