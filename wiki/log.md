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

## [2026-05-16] lint | 初回 lint パス

**修正したバグ**:
- [wiki/entities/Kozaneba.md](entities/Kozaneba.md) bootstrap 時の `../raw/` 相対パスが間違っており `wiki/raw/` に解決されていた → `../../raw/` に修正。「(未作成)」注記も削除。
- 5 ファイルで markdown link URL 内の括弧が未エスケープのため URL が `(開発` で打ち切られていた → `%28...%29` に URL エンコード:
  - Scrapboxこざね.md, 毛玉問題.md, Kozaneba_vs_Scrapbox.md, Scrapbox.md, 設計判断ログ.md
- 設計判断ログ.md で Accessism のソース日付が `2021-12-18` だったが実ファイルは `2021-12-19` → 修正
- Kozaneba_vs_Keichobot.md の `[表札をつけて束ねる]` リンクのページが存在しないため、リンクを通常テキストに(※後の lint パスで表札をつけて束ねるページは作成された)
- 設計判断ログ.md の `[wiki/.task2_files.txt](../.task2_files.txt)` 参照を削除(working file への参照)

**残存する dangling link 9 件**(=作成されていない概念ページへの参照、すべて原文の固有用語):
- [時間的スキーム.md](concepts/時間的スキーム.md) — [ねりねり](concepts/ねりねり.md), [Kozaneba_vs_Scrapbox](themes/Kozaneba_vs_Scrapbox.md) でも言及、raw に 32 回出現
- [モザイク状の世界.md](concepts/モザイク状の世界.md) — [探検ネット](concepts/探検ネット.md) から
- [情報の中に住む.md](concepts/情報の中に住む.md) — [探検ネット](concepts/探検ネット.md) から
- [thing.md](concepts/thing.md) — [dwell-think](concepts/dwell-think.md) から、raw に 28 回
- ~~[表札をつけて束ねる.md](concepts/表札をつけて束ねる.md)~~ ※後に作成済み
- [なめらかな畳まれ.md](concepts/なめらかな畳まれ.md) — [こざね法](concepts/こざね法.md) から
- [繰り返しKJ法.md](concepts/繰り返しKJ法.md) — [累積KJ法](concepts/累積KJ法.md) から

**lint 観察(未対応)**:
- raw に多数登場するが wiki に欠けている概念トップ(参照回数):
  - クリーンランゲージ 168(Keichobot の方法論の中核、独立 entity/concept にすべき)
  - 矢印 / 矢印機能 164(現在 [線を引く機能](concepts/線を引く機能.md) に統合されている可能性、別ページにすべきか要判断)
  - チュートリアル 100、表札 86、ブロードリスニング 74、探検ネット勉強会 44、リリースノート 38、コンテキスト的スキーム 16
  - エンティティ追加候補: いどばた 19、広聴AI 20、Miro 45、Audrey Tang 13、Gendlin 11、梅棹忠夫(出現あり、未集計)
- frontmatter `updated` フィールドはすべて `2026-05-16` で揃っている(初回生成)
- 孤立ページ(inbound link なし)は 0 件
- inbound link が極端に多いハブ: [Kozaneba](entities/Kozaneba.md), [Scrapbox](entities/Scrapbox.md), [Keichobot](entities/Keichobot.md) — 健全

## [2026-05-16] ingest | 頻出概念・エンティティ 21 ページ追加

サブエージェント1名に委託。lint で見つかった「raw 頻出だが未作成」概念と dangling link を解消するページを作成。

**追加した entities(6)**: [Miro](entities/Miro.md), [いどばた](entities/いどばた.md), [Audrey_Tang](entities/Audrey_Tang.md), [Gendlin](entities/Gendlin.md), [梅棹忠夫](entities/梅棹忠夫.md), [AIエンジニアたち](entities/AIエンジニアたち.md)

**追加した concepts(15)**: [時間的スキーム](concepts/時間的スキーム.md), [コンテキスト的スキーム](concepts/コンテキスト的スキーム.md), [表札をつけて束ねる](concepts/表札をつけて束ねる.md), [thing](concepts/thing.md), [モザイク状の世界](concepts/モザイク状の世界.md), [情報の中に住む](concepts/情報の中に住む.md), [なめらかな畳まれ](concepts/なめらかな畳まれ.md), [繰り返しKJ法](concepts/繰り返しKJ法.md), [クリーンランゲージ](concepts/クリーンランゲージ.md), [ブロードリスニング](concepts/ブロードリスニング.md), [広聴AI](concepts/広聴AI.md), [矢印機能](concepts/矢印機能.md)(線を引く機能の別名)、[物理演算](concepts/物理演算.md), [チュートリアル](concepts/チュートリアル.md), [探検ネット勉強会](concepts/探検ネット勉強会.md)

**既存ページへの最小相互リンク追加**: ねりねり, 線を引く機能, dwell-think, Keichobot, 体験過程, Plurality

**lint 2回目の追加修正**:
- 4 ファイルで新たに paren-in-URL バグを発見(探検ネット勉強会, 矢印機能, 表札をつけて束ねる, Miro)→ `%28%29` エンコード
- [wiki/index.md](index.md) を全面再構成(entities 13、concepts 48、themes 7 = 計 68 ページ + overview/index/log)

**最終 lint 結果**: wiki dangling 0 件、raw dangling 0 件

**残る懸念**:
- Glen Weyl, 華厳経, 荘子, マインドマップ の個別ページは未作成(必要に応じて)
- 概念マップ(いどばた/Plurality 周辺で頻出)の独立ページは未作成、辺ラベルとペアで作る余地あり

## [2026-05-16] query | 改善するか作り直すか

- nishio の問いかけ「改善するか作り直すか」を起点に、wiki(系譜 / なぜ作るのか / Canvas移行の検討)を読んで観察を提示:
  - 「2つの Kozaneba」議論で本人が既に二項対立を解体している
  - Canvas プロトは部分的に走っており、判断は「分けるか統合するか」
  - 動機レイヤと技術レイヤは独立に評価できる(動機は安定、技術はボトルネック)
- nishio の応答:「動機は変わっていないが技術が陳腐化したかもしれない」と認め、さらに 3 つの具体例を提示(N項関係の線 / 囲んで畳む / 辺ラベル無しの線)。「断片自体よりも関係性を上手く扱いたい」「紙時代の線を引く高コスト性が断片偏重を生んだ」と本人が言語化。
- 新規 theme ページ 2 本を作成:
  - [活用されなかった機能](themes/活用されなかった機能.md) — 「先回りした一般化」の共通パターンとして 3 事例を束ね、Canvas 化時の「移植しない決定」の判断材料として整理
  - [断片中心から関係中心へ](themes/断片中心から関係中心へ.md) — 紙の物理制約由来の断片偏重と、辺ラベル / 概念マップ / 広聴AI 連携で見える関係中心への重心移動
- [wiki/index.md](index.md) の themes セクションに 2 本を追加。
- 残る論点(未着手): 関係中心の Kozaneba は「Kozaneba の自然な進化」か「別物への分岐」か、KJ法 / こざね法 との接続をどう保つか、Scrapbox との棲み分け。

## [2026-05-16] ingest | raw/a.txt — 関係性UI設計 GPT 4ラウンド対話

- [raw/a.txt](../raw/a.txt) を読み込み。nishio と GPT の4ラウンドの対話で、関係性操作 UI を巡って Shneiderman / Yi-Kang-Stasko / Heer-Shneiderman / Nielsen / W3C link types / 空間ハイパーテキスト / ETable / brushing-linking / Nissenbaum / IBIS-gIBIS / Open Hypermedia / Wikidata / RDF 1.2 / Unfolding Edges / GraphRAG / Gendlin / クリーンランゲージ / Focusing を縦横に引きながら議論。
- nishio の問題提起ライン:
  - Round 2: 「リンクに意味がないから毛玉、リンクに意味記述するのが負担はノード偏重では? ノードのコンテンツの多くは本来エッジに置かれるべきでは?」
  - Round 3: 「リンクは1対1を仮定するが N項かも、Gendlin の関係と側面の議論と照らせ」
  - Round 4: 「クリーンランゲージのような会話的明瞭化との関連、ユーザは『操作』を言うのではなく質問への回答とボタンで少しずつ表出する」
- GPT 側の主要な合成:
  - 毛玉の3段階定義(視覚→意味潰し→分節過程の消去)
  - ノード偏重批判、「ノード本文の関係負債」概念、関係を第一級オブジェクトに
  - N項関係でも足りない → 関係場(Situation → Aspect → Relation Process → Projection)
  - LLM は決定者ではなく「持ち上げ補助者 / 配置差分提示者」
  - Clean Relation Elicitation(4パネル: Situation Pad / Aspect Tray / Relation Field / Projection、5操作: Lift/Gather/Formulate/Project/Carry forward、2ボタン種: 注意誘導/フィット確認)
- **新規 4 ページ**:
  - [concepts/関係場.md](concepts/関係場.md) — Gendlin 系の中心概念、三層モデル、「まだ語がない」の保存
  - [concepts/N項関係.md](concepts/N項関係.md) — 既出だが未作成だった概念、関係場との関係も整理
  - [themes/関係を第一級にする.md](themes/関係を第一級にする.md) — ノード偏重批判 + 外部系譜(Open Hypermedia / Wikidata / RDF 1.2 / W3C N-ary / IBIS / Unfolding Edges / ハイパーグラフ DB)+ Kozaneba 設計含意
  - [themes/Clean_Relation_Elicitation.md](themes/Clean_Relation_Elicitation.md) — クリーンランゲージ系 UI 設計案、Keichobot/Kozaneba 分業の再定義
- **更新 7 ページ**:
  - [themes/断片中心から関係中心へ.md](themes/断片中心から関係中心へ.md) に「ノード本文の関係負債」「既存系譜」セクション追加
  - [themes/活用されなかった機能.md](themes/活用されなかった機能.md) に N項関係 UI 失敗の深い理由(項先行モデル)追加
  - [concepts/辺ラベル.md](concepts/辺ラベル.md) に N項→関係場発展、Clean Relation Elicitation 代替案追加
  - [concepts/線を引く機能.md](concepts/線を引く機能.md) に「線」概念そのものの限界追加
  - [concepts/クリーンランゲージ.md](concepts/クリーンランゲージ.md) に関係性 UI への応用追加(2023-03 の「ボードゲームとしての CL」逆説への応答)
  - [concepts/毛玉問題.md](concepts/毛玉問題.md) に毛玉の3段階定義(視覚→意味潰し→分節過程消去)
  - [entities/Keichobot.md](entities/Keichobot.md) に関係場外化パイプラインの上流位置付け追加
- [wiki/index.md](index.md) を更新: concepts に N項関係・関係場、themes に 2 本追加
- 主要発見:
  - **毛玉問題の再定義**: 単なる視覚問題ではなく、二項投影による分節過程の消去。Kozaneba の長年の課題への根本的視座を獲得
  - **「ノード本文の関係負債」**: nishio の長年の Kozaneba 設計判断(N項関係 UI、辺ラベル無し、囲んで畳む)が、ノード偏重という構造的バイアスの症状として整理できる
  - **Keichobot/Kozaneba 分業の連続体化**: 従来の「言語化 vs 一次元化」は離散的な分業だったが、「Situation → Aspect → Relation Field → Projection」の連続体の中の上流/下流として再配置可能
  - 関係性 UI 設計の核心は **保存モデル(リッチ)と表示モデル(用途別射影)の分離** にあり、エッジを単に表示拡張しても破綻する
- 残る論点(未着手):
  - 既存 Kozaneba を改造するか Canvas プロト側に新規実装するか
  - 関係場 UI と [広聴AI](concepts/広聴AI.md) / [Plurality](concepts/Plurality.md) の大規模意見集約との接続
  - ボタン名・クリーン質問の英日翻訳問題
  - 「[ボードゲームとしてのクリーンランゲージ](concepts/クリーンランゲージ.md)」(2023-03)の逆説(LLM で自然にするほど構造が見えない)への対応として Clean Relation Elicitation が機能するかの検証

## [2026-05-16] query | 「Situation → Aspect → Relation Field → Projection」の解説要求 → fill back

- nishio が [関係場](concepts/関係場.md) のパイプラインの解説を要求。チャット上で具体例(哲学書読解 / Keichobot 対話)、Kozaneba 現状との対応表、「逆順(項は後で取り出される)」の本質を展開。
- nishio 「まだ理解しきれていないが、重要と思うので fill back しといて」→ チャットの解説内容を [関係場](concepts/関係場.md) に追記。
- 追加セクション:
  - **「逆順」の本質** — 普通のグラフモデル vs ジェンドリン的順序の対比、項が後で取り出される意味
  - **具体例** — 哲学書読解(Gendlin/Heidegger/川喜田の並列)、Keichobot 対話(「なぜ作るのか」の発見)
  - **Kozaneba の現状との対応** — こざね = Situation、近接配置/グループ化 = Aspect、線 = いきなり Projection(関係場をスキップしている)
  - **各層の境界は揺れる** — 行き来する作業空間として機能

## [2026-05-16] query | こざねと源の長文(Situation の欠落)

- nishio の観察: 「Kozaneba のこざねは Scrapbox のページタイトルに相当するもので、現状のこざねは『ページ本文』に相当するものを持っていない。一方で Scrapbox の『タイトル/本文 1:1』もおかしくて、1 つの長文から N 個の短文(=タイトル/こざね)が生えていると思う」
- これを [関係場](concepts/関係場.md) Pipeline 上にマップ:
  - Kozaneba: Aspect(こざね)+ Projection(キャンバス)— Situation を欠く
  - Scrapbox: Situation + Aspect は持つが 1:1 でカーディナリティが間違っている
  - 実体: 1 長文 → N こざね
- 関連して、梅棹こざね と Kozaneba こざね の定義のズレが Pipeline 上の位置の違いとして整理できる(梅棹 ≒ Relation Field、Kozaneba ≒ Aspect)。両者とも Situation を持たない点で共通の盲点。
- 外部の類似物(Andy Matuschak の evergreen notes、Roam/Logseq の block model、outliner)はいずれも長文と短文の関係をデータモデルで保持している。Kozaneba には欠落。
- 新規 concept [源の長文](concepts/源の長文.md) を作成。データモデル案として `source_situation` フィールド(text + anchor)を提示。
- [こざね](concepts/こざね.md) に「Pipeline 上の位置: 梅棹こざね との定義のズレ」「源の長文を持たない問題」セクション追加。
- [wiki/index.md](index.md) を更新。
- 観察ポイント: [断片中心から関係中心へ](themes/断片中心から関係中心へ.md) と [源の長文](concepts/源の長文.md) は、それぞれ断片偏重の「横の問題(関係性)」と「縦の問題(由来)」を扱う。両方が「断片中心バイアス」の異なる症状。
