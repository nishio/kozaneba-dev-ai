---
title: Log
type: meta
created: 2026-05-16
updated: 2026-05-16
---

時系列の作業ログ。append-only。新しいエントリはファイル末尾に追加する。見出しは `## [YYYY-MM-DD] <action> | <subject>` の形式で統一する(`grep "^## \[" wiki/log.md` でパース可能にするため)。

## [2026-05-16] bootstrap | wiki セットアップ

- [raw/init.txt](../raw/init.txt) と [llm-wiki.md](../llm-wiki.md) を読み、wiki パターンの採用を決定。
- [raw/nishio.json](../raw/nishio.json)(Scrapbox /nishio エクスポート、25633 ページ)から "Kozaneba" または "こざねば" を含む 403 ページを抽出し [raw/scrapbox_kozaneba/](../raw/scrapbox_kozaneba/) に保存。`_manifest.json` も同梱。
- ディレクトリ構成を作成:`wiki/{entities,concepts,themes,sources}/`。
- [CLAUDE.md](../CLAUDE.md) を作成(wiki スキーマと運用ルール)。
- 初期 wiki ページを作成:
  - [wiki/overview.md](overview.md) — Kozaneba 全体像
  - [wiki/entities/Kozaneba.md](entities/Kozaneba.md) — 中心エンティティ
  - [wiki/index.md](index.md) — ページカタログ
  - [wiki/log.md](log.md) — このログ

## [2026-05-16] ingest | 64 本の非日記タイトル一致ページ → entities/concepts/themes

- サブエージェント1名に委託(Task 1)。タイトルに "Kozaneba" を含み「Kozaneba開発日記」ではない 64 ページを全部読み、entities/ concepts/ themes/ を整備。
- 作成:
  - entities/ 6 本: [Regroup](entities/Regroup.md), [Movidea](entities/Movidea.md), [Keichobot](entities/Keichobot.md), [Scrapbox](entities/Scrapbox.md), [川喜田二郎](entities/川喜田二郎.md), [Devin](entities/Devin.md)
  - concepts/ 33 本: KJ法系・思考メタ概念・Gendlin系・思想家系・Kozaneba固有機能・使う行為(詳細は [index.md](index.md))
  - themes/ 6 本: [なぜ作るのか](themes/なぜ作るのか.md), [系譜](themes/系譜.md), [Kozaneba_vs_Scrapbox](themes/Kozaneba_vs_Scrapbox.md), [Kozaneba_vs_Keichobot](themes/Kozaneba_vs_Keichobot.md), [哲学書読解の実験](themes/哲学書読解の実験.md), [Canvas移行の検討](themes/Canvas移行の検討.md)
- 主要発見:
  - 「他の人に使われて成長する」が 2021-08 以降の通奏低音
  - 線を引く機能は 2021-12 の哲学書読解で「必要不可欠」と判定
  - 2025 に「2 つの Kozaneba」認識(人間規模 vs 広聴 AI リーフノード規模)
  - A型=Kozaneba / B型=Scrapbox の分業構想(2024、未実現)
  - 辺ラベルが 2025 年最大の機能ニーズ

## [2026-05-16] ingest | 2021 年の 102 本 → themes/設計判断ログ.md

- サブエージェント1名に委託(Task 2)。2021 年に作成された Kozaneba 関連 102 ページ(うち日記 39 本、本文言及 63 本)を全部読み、月別タイムラインに整理。
- 作成: [wiki/themes/設計判断ログ.md](themes/設計判断ログ.md) (318 行)
- 抽出した通奏低音テーマ:
  - 「自分のため」 vs 「多くの人のため」(8/8 で決着 → 9/3 揺り戻し → 12/25 で再整理)
  - Keichobot との分業: 「言語化」と「一次元化」(12/24 結晶化)
  - Scrapbox との関係: 「蓄える場 / 削っていく場」(8/28 明文化)
  - 近接 vs 線: 既存の構造を保ったまま新しい構造を入れる(8/26→9/6→10/4→12/8)
  - 「不確実性削減 vs 楽さ vs 面白さ」ドリブン(8/28 自己観察)
  - 子供のメタファー / Accessism
  - ユーザ拡張性を初期から重視(localStorage.onLoad → user_menus → UserScript)
  - テストファースト + 内部状態直接セット API(Cypress 思想)
- 未解決の問い: 矢印編集 UI、タッチデバイス対応、Ba をまたぐ RELEVANCE 発見、辺ラベル(三項関係)、物理演算の位置付け、大量こざね時のパフォーマンス、多言語化、VR/メタバース対応、自動単語こざね化アルゴリズム

## [2026-05-16] index | 全 47 ページを index.md に登録

- [wiki/index.md](index.md) を更新し、entities 7・concepts 33・themes 7 を分類して列挙
- 既知の dangling link:
  - [時間的スキーム.md](concepts/時間的スキーム.md) — ねりねりページから参照されているが未作成
