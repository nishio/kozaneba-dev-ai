---
title: Index
type: meta
created: 2026-05-16
updated: 2026-06-03
---

このリポジトリの wiki ページ一覧。新しいページを作るたびに更新する。設計方針は [CLAUDE.md](../CLAUDE.md) を参照。

## Overview

- [overview.md](overview.md) — Kozaneba 全体像(入口)

## Entities(プロダクト・ツール・人)

### プロダクト
- [Kozaneba](entities/Kozaneba.md) — かんがえをまとめるデジタル文房具(本プロジェクトの中心)
- [Regroup](entities/Regroup.md) — Kozaneba の前々身
- [Movidea](entities/Movidea.md) — Regroup を書き直した直前身、後に Kozaneba にリネーム
- [Keichobot](entities/Keichobot.md) — クリーンランゲージ系の対話ボット、Kozaneba と「言語化/一次元化」で分業
- [Scrapbox](entities/Scrapbox.md) — nishio のもう一つの中核ノートツール
- [Miro](entities/Miro.md) — Kozaneba の代替候補・参考実装(2022 デジタル KJ法 のデファクト)
- [いどばた](entities/いどばた.md) — AI 対話による言語化システム、2025 で Keichobot 的コーチング実装

### 人物
- [川喜田二郎](entities/川喜田二郎.md) — KJ法/こざね法/探検ネット/累積KJ法の考案者
- [梅棹忠夫](entities/梅棹忠夫.md) — こざね法の考案者(川喜田と並ぶ)
- [Gendlin](entities/Gendlin.md) — 体験過程・フェルトセンス・dwell-think の出典
- [Audrey_Tang](entities/Audrey_Tang.md) — Plurality の共著者

### AI ツール
- [Devin](entities/Devin.md) — 2025 年に投入された自律型 AI エンジニア
- [AIエンジニアたち](entities/AIエンジニアたち.md) — o1 Pro / GPT-5 / Claude / Devin の横断整理

### 開発・運用ツール
- [Sentry](entities/Sentry.md) — production エラーとユーザ feedback の観測ソース

## Concepts(概念)

### 方法論(KJ法系)
- [KJ法](concepts/KJ法.md) — 川喜田二郎の発想法
- [こざね法](concepts/こざね法.md) — 梅棹忠夫由来、KJ法と並ぶ
- [こざね](concepts/こざね.md) — 情報の単位
- [探検ネット](concepts/探検ネット.md) — 川喜田の方法論
- [探検ネット勉強会](concepts/探検ネット勉強会.md) — 2022-06 連続勉強会、Kozaneba 設計の派生源
- [累積KJ法](concepts/累積KJ法.md)
- [繰り返しKJ法](concepts/繰り返しKJ法.md) — 川喜田自身もほぼ実践しなかった
- [B型文章化](concepts/B型文章化.md) — KJ法の文章化工程
- [表札をつけて束ねる](concepts/表札をつけて束ねる.md) — KJ法 の中核操作、Kozaneba 未対応
- [渾沌をして語らしめる](concepts/渾沌をして語らしめる.md) — 川喜田由来
- [花火日報](concepts/花火日報.md)
- [なめらかな畳まれ](concepts/なめらかな畳まれ.md) — ズームによる代替手法
- [大きな付箋](concepts/大きな付箋.md) — なめらかな畳まれ に対する能動的対抗手段、人力 inverse-zoom title

### 思考のメタ概念
- [ねりねり](concepts/ねりねり.md) — 構造の破壊と再生
- [既存の構造の破壊](concepts/既存の構造の破壊.md)
- [時間的スキーム](concepts/時間的スキーム.md) ⇔ [コンテキスト的スキーム](concepts/コンテキスト的スキーム.md) — 一対の見方
- [一次元化](concepts/一次元化.md) — Keichobot との対比
- [主客分離](concepts/主客分離.md)
- [連想的雰囲気](concepts/連想的雰囲気.md)
- [毛玉問題](concepts/毛玉問題.md)
- [思い出し効果](concepts/思い出し効果.md) — Scrapbox 由来
- [情報の中に住む](concepts/情報の中に住む.md) ⇔ [モザイク状の世界](concepts/モザイク状の世界.md) — 川喜田の一対

### Gendlin 由来(体験過程理論)
- [体験過程](concepts/体験過程.md)
- [フェルトセンス](concepts/フェルトセンス.md)
- [平行的シンボル](concepts/平行的シンボル.md)
- [dwell-think](concepts/dwell-think.md)
- [thing](concepts/thing.md) — Heidegger 由来、Gendlin 経由

### 思想家・哲学由来
- [華厳](concepts/華厳.md) — 哲学書読解で得た概念
- [コンヴィヴィアリティ](concepts/コンヴィヴィアリティ.md) — Illich
- [Plurality](concepts/Plurality.md) — Audrey Tang / Glen Weyl
- [ブロードリスニング](concepts/ブロードリスニング.md) — Plurality 由来
- [広聴AI](concepts/広聴AI.md) — 2025、Kozaneba との「2つの」議論
- [密度の高さを大きさに変換して可視化](concepts/密度の高さを大きさに変換して可視化.md) — 2025-08 Canvas プロトタイプの中心可視化原則
- [内部構造がわかりやすい](concepts/内部構造がわかりやすい.md) — 可視化評価の肌感メトリック、機能 vs 見栄えのトレードオフ事例
- [IOFI](concepts/IOFI.md)
- [クリーンランゲージ](concepts/クリーンランゲージ.md) — Keichobot の方法論の中核

### Kozaneba 固有の機能・概念
- [線を引く機能](concepts/線を引く機能.md) / [矢印機能](concepts/矢印機能.md)(別名)
- [辺ラベル](concepts/辺ラベル.md) — 2025年の最大の機能ニーズ
- [N項関係](concepts/N項関係.md) — 1:N、(始点, 関係, 終点)、超多項関係
- [関係場](concepts/関係場.md) — Gendlin系。N項関係を超える関係表現の中心単位
- [源の長文](concepts/源の長文.md) — こざねが生えてくる Situation。現状の Kozaneba データモデルに欠けている
- [物理演算](concepts/物理演算.md) — 「機械が動かしてはいけない」葛藤
- [Scrapboxこざね](concepts/Scrapboxこざね.md)
- [UserScript](concepts/UserScript.md)
- [チュートリアル](concepts/チュートリアル.md) — UX 設計の中核ジレンマ

### Kozaneba を使う行為・態度
- [Kozaneba読書](concepts/Kozaneba読書.md)
- [Kozaneba修行](concepts/Kozaneba修行.md)
- [推敲](concepts/推敲.md)
- [竹の足場](concepts/竹の足場.md)
- [ドッグフーディング](concepts/ドッグフーディング.md)
- [締め切りドリブン](concepts/締め切りドリブン.md)

## Themes(複数ソース横断の統合)

- [なぜ作るのか](themes/なぜ作るのか.md) — 動機の変遷(自分のため→他人に使われて成長→人類の知的能力強化)
- [系譜](themes/系譜.md) — grouping → Regroup → Movidea → Kozaneba → Canvas検討
- [Kozaneba vs Scrapbox](themes/Kozaneba_vs_Scrapbox.md) — 削っていく場 vs 蓄える場
- [Kozaneba vs Keichobot](themes/Kozaneba_vs_Keichobot.md) — 一次元化 vs 言語化
- [哲学書読解の実験](themes/哲学書読解の実験.md) — 2021-12〜2022-01
- [Canvas移行の検討](themes/Canvas移行の検討.md) — 2025-08 の Devin/GPT-5 議論
- [活用されなかった機能](themes/活用されなかった機能.md) — N項関係 / 囲んで畳む / 辺ラベル無しの線を「先回りした一般化」として束ねる
- [断片中心から関係中心へ](themes/断片中心から関係中心へ.md) — 紙の物理制約由来の断片偏重と、関係中心への重心移動
- [関係を第一級にする](themes/関係を第一級にする.md) — ノード偏重批判と関係オブジェクトの属性モデル、既存系譜(Open Hypermedia / Wikidata / RDF / IBIS / Unfolding Edges)
- [Clean Relation Elicitation](themes/Clean_Relation_Elicitation.md) — クリーンランゲージ系UIで関係場を漸進的に外化する設計案、Keichobot/Kozaneba 分業の再定義
- [設計判断ログ 2021](themes/設計判断ログ.md) — 2021年の月別タイムラインと通奏低音
- [3 Plan 議論](themes/3plan議論.md) — 2026-05-16 の「Kozaneba の次に何を作るか」議論。Plan B(現Kozaneba改造、自分の作業加速)+ Plan A(Keichobot+いどばた+Kozaneba参考の新規サービス)の二段構えに着地
- [3 つのストーリー比較](themes/3つのストーリー比較.md) — 改善 / 似たものを新規作成 / 全く新しいもの、の3案を比較し、各 MVP と分岐条件を整理
- [データモデル刷新の選択肢](themes/データモデル刷新の選択肢.md) — 1 巨大 JSON からの脱出、Yjs/Automerge/Loro/Zero/SQLite-WASM/CAS の比較、Loro ホスティング 3 ルート(Firestore + binary / Cloudflare DO+R2 / 自前 Hono+Postgres)
- [線UIサーベイ 2026](themes/線UIサーベイ_2026.md) — Miro/FigJam/tldraw/Kinopio/Tana 他 16 ツールの connection 描画パターン分類と Kozaneba 改修案
- [畳むUIの再設計](themes/畳むUIの再設計.md) — Pad++/Bret Victor/Heptabase/Magic Lens/AI 自動表札による「囲んで畳む」の刷新案
- [Canvas 1 万件デモの拡張](themes/Canvas_1万件デモの拡張.md) — 2025-08 の 1 万枚 Canvas プロトタイプ(本流ではないサイド実験、`#/clusters` サブルートに cluster sticky の draft が残ったまま 2025-08-29 にクラスタ抽出方針 / マージ閾値 / 路線統合可否が未解決のまま開発停止)を 2026-06 サーベイと突き合わせ、AI ペアプロで実装は速いが設計判断は残ることを整理
- [認知メタファのデザイン](themes/認知メタファのデザイン.md) — 既知メタファを借りる、抽象度を上げない、という Kozaneba 設計哲学。散布図/KDE/付箋の比較から内部構造・見栄え・既知メタファの 3 軸トレードオフを抽出
- [人間が動かすから隙間ができる](themes/人間が動かすから隙間ができる.md) — 本流(人間駆動の小規模)とプロトタイプ(アルゴ駆動の大規模)で「空間の隙間の意味」が違うことを示し、frame 抽象を機械的に統一しない理由を提供。物理演算 禁忌と同系統
- [Plan B 試行 2026-06](themes/Plan_B試行_2026-06.md) — 辺ラベル UI を AI Agent に投げて「賢い AI Agent は過去実装の悪いところを簡単に直す」仮説を検証する単発実験。人間体験で実装ノーオペ発覚 → 線UI全体の再設計と spec/検証フレームの再構築へ
- [線UI 再設計 2026-06](themes/線UI再設計_2026-06.md) — hover anchor + 編集 window + 離脱 commit の統一設計。ラベル/線種/相手間違いの修正動線を「commit 瞬間」を起点に統合
- [AI 委託の設計と検証 2026-06](themes/AI委託の設計と検証_2026-06.md) — Plan B 試行から抽出した、spec writer (LLM) / AI Agent (Codex) / 自動テスト / 人間検証の協業分担と spec template
- [Kozaneba テスト基盤調査 2026-06](themes/Kozanebaテスト基盤調査_2026-06.md) — CI 未接続、Cypress 既存ベースライン失敗、Firebase emulator 必要性、状態モデル中心のキャンバステスト戦略、UI テスト helper 整備方針の整理
- [テスト改善計画](themes/テスト改善計画.md) — PR #40〜#44 で CI / Vite 移行まで完了後、#17 を座標変換・hit test・undo/redo・保存復元などへ分解し、実測済みの旧 Movidea spec 棚卸しへ接続する計画
- [CI安定化とVite移行 2026-06](themes/CI安定化とVite移行_2026-06.md) — 座標ロジック単体テスト、required CI、Node 24、CRA から Vite への移行、production console log cleanup を PR #40〜#45 で完了した実施記録と学び
- [AI生成Issueのトリアージ](themes/AI生成Issueのトリアージ.md) — AI が生成した一般改善 Issue を、コード事実・実行単位・計測前提・製品方針で選別し、Plan B の実行可能 backlog に変換する判断基準
- [運用エラー観察](themes/運用エラー観察.md) — Sentry の production error / feedback を Plan B の観測ソースとして読むための分類規準
- [iPad実機対応調査 2026-06](themes/iPad実機対応調査_2026-06.md) — mouse event 前提の現行キャンバス実装、touch/pointer 対応の問題候補、iPad 実機 smoke test 手順と完了条件

## Sources(個別ソースの要約)

- [llm-wiki クロスリファレンス](sources/llm-wiki-cross-reference.md) — 並行 wiki [llm-wiki](../../llm-wiki/wiki/index.md) との対応表。Kozaneba 観察から llm-wiki 側で抽出された概念群と、未取り込み候補
- [Kozaneba git history 2025 要約](sources/kozaneba-git-history-2025.md) — `work/kozaneba` の `git log` を読んだ、2025 年の主要な実装・設計変更の要約
- [Kozaneba コード構造調査 2026-05](sources/kozaneba-code-architecture.md) — `work/kozaneba/` 現コードの構造、Item/Annotation スキーマ、物理演算実装、辺ラベルや N項関係がスキーマ上は実装済みである事実
- [キャンバス状態テスト戦略 2026-06](sources/canvas-state-testing-strategy-2026-06.md) — ピクセル単位 E2E ではなく world 座標・状態モデル・イベント列を主対象にするキャンバスアプリのテスト設計メモ
- [Movidea legacy test inventory 2026-06](sources/movidea-legacy-test-inventory-2026-06.md) — Java 21 + Firebase emulator で旧 Movidea Cypress 23 specs を実測し、promote / rewrite-before-decision / delete に分類した棚卸し
- [静的 HTML export MVP 2026-06](sources/static-html-export-mvp-2026-06.md) — Ba JSON と軽量 read-only viewer を 1 ファイル HTML に埋め込む `Download Static HTML` 実装(PR #47)の記録
- [Release Notes / フォーラム 2021-2025 要約](sources/release-notes-2021-2025.md) — kozaneba-forum (英 17 ページ) と kozaneba-forum-jp (日 48 ページ) の全件を読んだ要約。EN/JP の差分、機能カテゴリ別タイムライン、外部ユーザ一覧、公開された設計原則(「default で線が増えない方を選ぶ」「線を click 可能にすると ドラッグ が妨害される」)を抽出

## Meta

- [CLAUDE.md](../CLAUDE.md) — wiki スキーマと運用ルール
- [llm-wiki.md](../llm-wiki.md) — 採用しているパターン(外部参照)
- [log.md](log.md) — 作業ログ(時系列)
