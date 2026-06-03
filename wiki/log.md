---
title: Log
type: meta
created: 2026-05-16
updated: 2026-06-03
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

## [2026-05-16] ingest | raw/関係UI議論_GPT.md — 関係性UI設計 GPT 4ラウンド対話

- [raw/関係UI議論_GPT.md](../raw/関係UI議論_GPT.md) を読み込み。nishio と GPT の4ラウンドの対話で、関係性操作 UI を巡って Shneiderman / Yi-Kang-Stasko / Heer-Shneiderman / Nielsen / W3C link types / 空間ハイパーテキスト / ETable / brushing-linking / Nissenbaum / IBIS-gIBIS / Open Hypermedia / Wikidata / RDF 1.2 / Unfolding Edges / GraphRAG / Gendlin / クリーンランゲージ / Focusing を縦横に引きながら議論。
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

## [2026-05-16] ingest | llm-wiki からのクロスリファレンス作成

- nishio の依頼で並行 wiki [/Users/nishio/llm-wiki/wiki/](../../llm-wiki/wiki/index.md) を走査し、Kozaneba プロジェクトに関係するページを抽出。
- llm-wiki 側は 2026-04 以降に育てられた知的生産・LLM Wiki パターンの知識ベース。Kozaneba を比較対象として継続参照しており、Kozaneba 観察から抽出された概念群がすでに存在することを確認。
- 取り込み方針について nishio に質問 → 「まず全体マッピングを wiki/sources/ に 1 枚作る」を選択。
- 作成: [wiki/sources/llm-wiki-cross-reference.md](sources/llm-wiki-cross-reference.md) — llm-wiki 側ページ群と Kozaneba 側ページの対応表、未取り込み候補概念リスト、取り込み方針メモを記載。
- [wiki/index.md](index.md) の Sources セクションに 1 件追加(初の sources/ エントリ)。
- 主要発見:
  - llm-wiki 側に **Kozaneba を直接言及するページ** 4 本(entities/kozaneba, entities/connecting-dots, entities/mindtrellis, sources/gpt-mindtrellis-connectingdots-20260503)
  - llm-wiki 側で **Kozaneba 観察から抽出された一般化概念** 3 本(pre-linguistic-structuring / kangaeru-hanabi / structure-as-hypothesis)
  - [themes/関係を第一級にする](themes/関係を第一級にする.md) と llm-wiki 側 `relation-flattening` / `concept-as-region` がほぼ同じ問題圏(KJ 法・Gendlin・埋め込み議論)で、相互に強い理論的補強関係にある
  - llm-wiki は Kozaneba 側の議論を観察した結果として育っているため、同じ素材で抽象化が一段進んでいる場合が多い(逆輸入の価値あり)
- 次の候補(未着手): 個別ページの取り込み判断(「前言語的構造化」を独立 concept ページ化するか、[Kozaneba読書](concepts/Kozaneba読書.md) への追記で済ますか等)、「AI を Kozaneba に入れるなら」テーマページの新設可能性(structure-as-hypothesis / ConnectingDots の Inbox 化が核)。

## [2026-05-16] query | 3 Plan 議論 → 道具改造 + 新規プロダクトの二段構え

- nishio が「今考えていること」として 3 Plan を提示:
  - Plan A: Keichobot 発展形(対話による個人向け言語化支援)
  - Plan B: 300 件文章からの知識獲得支援(直近の出口あり)
  - Plan C: Kozaneba読書(「自分の中に価値」を信じられない人への外発入口)
- Claude 側の最初の整理: 3 Plan は排他ではなく、[源の長文](concepts/源の長文.md)(Situation データモデル)拡張を共通基盤として[関係場](concepts/関係場.md) Pipeline 上の異なる Situation 由来として並べられる。C は B の特殊ケースとして包含。
- nishio の回答で構図が転換:
  - 「直近の出口」= **nishio 自身の 300 件作業を加速**(= Plan B)
  - 「Plan A 時の Keichobot/いどばた との関係」= **3 つを参考にしつつ全く新しいサービス**
- 結果: 3 Plan の選択問題が「**道具改造(Plan B)+ 新規プロダクト(Plan A)の二段構え**」に再定式化。長年の「改善するか作り直すか」の問いに対して**両方を並行する**結論。Plan B が「Plan A への踏み台」になる。
- Plan A の輪郭素描(llm-wiki 側の概念を組み合わせ):
  - [ConnectingDots](../../llm-wiki/wiki/entities/connecting-dots.md) の最初の本実装になる可能性
  - Dots/Relations/Stories/Views 4 層 + identity-without-name + pre-linguistic-structuring + structure-as-hypothesis + relation-flattening 回避
  - = 対話で Situation を引き出し、AI が候補化、人間が空間で確定、Stories として読み筋を立てるシステム
- 新規 theme [3 Plan 議論](themes/3plan議論.md) を作成。Plan B 期間で記録すべき「Plan A 要求発見」5 項目(現Kozanebaでできなかった操作 / AIと対話したかった場面 / 言語化が早すぎて諦めた関係 / 同じこざねを別Storyで再利用したかった場面 / 1長文→Nこざね経路)を含む。
- [wiki/index.md](index.md) の themes に追加。
- Open Questions: deprecate vs 並列、Plan B 期間の目安、MVP の輪郭、Plan A の専門家ユーザ像、Plan C の入口戦略化。

## [2026-05-16] fill back | 3 Plan 議論を関連 8 ページに浸透

- nishio の依頼で、[3 Plan 議論](themes/3plan議論.md) の核心(「両方やる」= 道具改造の Plan B + 新規サービスの Plan A、3 系列を吸収する合流点としての Plan A、源の長文を共通基盤とする等)を関連既存ページに局所追記。
- 更新したページ 8 本:
  - [overview.md](overview.md) — 「改善するか作り直すか」の問いに「両方を並行する」方針を明記。Plan B/A の概要追加
  - [entities/Kozaneba.md](entities/Kozaneba.md) — 新規セクション「現状(Plan B)と次世代(Plan A)の二段構え」追加
  - [themes/系譜.md](themes/系譜.md) — 系譜図に 2026-05-16 の分岐を追加(Plan B + Plan A)、新規セクション「2026-05-16: Plan B + Plan A への分岐」追加。Plan A は 3 系列を吸収する合流点と明示
- [themes/Canvas移行の検討.md](themes/Canvas移行の検討.md) — 新規セクション「2026-05-16: 『両方やる』への着地」追加。Canvas 化を急ぐ理由が弱まったことを明記
- [themes/Kozaneba vs Keichobot](themes/Kozaneba_vs_Keichobot.md) — 新規セクション「2026-05-16: Plan A による分業の解体」追加。連続体としての 1 システム化が Plan A の輪郭

## [2026-05-19] query | Kozaneba git log の主要設計変更

- `work/kozaneba/` を一次ソースとして `git log` と主要 commit の diff/stat を確認。
- 作成: [wiki/sources/kozaneba-git-history-2025.md](sources/kozaneba-git-history-2025.md)
- 主要整理:
  - 2025-04 は React/Firebase 更新、Netlify・lockfile・`useEffect` 修正による「生存性の回復」
  - 2025-08〜09 は Selection menu 強化、merge の意味変更、線ラベル追加による「探索操作と関係操作の再強化」
  - 線ラベルはデータモデル上は前進したが、入力 UX は未確定のまま揺れている
- [wiki/index.md](index.md) の Sources に登録。

## [2026-05-19] query | Kozaneba の次に進む 3 ストーリー比較

- nishio の依頼で、「改善」「似たものを新しく作る」「全く新しいものを作る」の 3 案を、Kozaneba の歴史を踏まえて比較。
- 作成: [wiki/themes/3つのストーリー比較.md](themes/3つのストーリー比較.md)
- 含めた内容:
  - 3 案の比較表(守るもの / 捨てるもの / 解決対象 / リスク)
  - 各案の MVP
  - どの観察が出たらどの案へ進むべきかの判断基準
  - 「Aで観察を取り、Bを本命候補に育て、Cを入口戦略として温存する」という暫定見立て
- [wiki/index.md](index.md) の Themes に登録。

## [2026-05-19] fill back | 3つのストーリー比較を関連 5 ページに浸透

- [overview.md](overview.md) — 3 ストーリーの要点と暫定順序を追記
- [entities/Kozaneba.md](entities/Kozaneba.md) — Kozaneba を「現役プロダクト + 次世代設計の観察装置」として再位置づけ
- [themes/Canvas移行の検討.md](themes/Canvas移行の検討.md) — Canvas を最上位目標ではなく、3 ストーリーのどこで必要かを見極める対象として整理
- [themes/関係を第一級にする.md](themes/関係を第一級にする.md) — relation-first 設計が特に「似たもの新規」ストーリーの中核だと明記
- [themes/3plan議論.md](themes/3plan議論.md) — 二段構えの整理と、後続の 3 ストーリー比較ページとの役割分担を追記
  - [themes/関係を第一級にする](themes/関係を第一級にする.md) — Kozaneba 設計への含意の末尾に、Plan A が本テーマの直接の実装ターゲットになる旨を追加(relation-flattening 回避まで含む)
  - [themes/Clean Relation Elicitation](themes/Clean_Relation_Elicitation.md) — Open Questions の選択肢に Plan A を追加(2026-05-16 時点で有力)
  - [concepts/源の長文](concepts/源の長文.md) — 新規セクション「Plan B と Plan A の共通基盤としての位置」追加。Plan B 期間の先行実装基盤として位置づけ
- 効果:
  - 3 Plan 議論の影響が wiki ネットワーク全体に拡散し、どのページから入っても Plan A/B の二段構えに到達できる
  - 既存ページに残っていた Open Question(「Canvas 化するか」「Keichobot/Kozaneba 融合」「移植先での書き直し」)が Plan A の登場で形を変えたことが明示
  - Plan B 期間中の作業で他のページを開いたときに、Plan A 要求発見の文脈を保ち続けられる

## [2026-05-25] query | work/kozaneba コード構造調査

- nishio の依頼で `work/kozaneba/`(main, 5de81c2)のソース構造を直接読み、Scrapbox メモには現れない実装上の事実を確認。
- 主要発見:
  - Item Union は **`kozane / group / scrapbox / gyazo` の 4 種のみ**。「源の長文」を持つ型は無く、`RTKozaneItem` も `text / position / scale / custom.{ style?, url? }` のみ
  - Annotation は **`line` 1 種類のみ**で、`items: Array(RTItemId)` で N項可、`label: String.optional()` で **辺ラベルのデータモデルは既に存在**(2025-09 PR #36 で入っている)
  - 線種は `heads: ("none" | "arrow")[]` と `is_doubled: Boolean` の組合せで全パターン表現
  - エッジクリック不可は `AnnotationLayer.tsx` の `pointerEvents: "none"` + 個別 `<line>` の `is_clickable` 復活設計に由来
  - 物理演算は `ItemRepulse`(全ペア走査 O(N²))+ `LineSpring`(線の重心ばね、自然長 KOZANE_WIDTH)+ `pin` の素朴実装
  - 状態管理は `reactn` の単一グローバル state、`package.json` の `name` は依然 `"movidea"`
  - 隠しフラグ `kozaneba.constants.exp_no_adjust` で「`#` 始まりを見出し風に表示」する実験機能

## [2026-05-25] fill back | コード構造調査を関連 6 ページに浸透

- 新規 source ページ [sources/kozaneba-code-architecture.md](sources/kozaneba-code-architecture.md) を作成し、コード一次調査の事実をまとめて [index.md](index.md) Sources に登録。
- 既存ページへの局所追記:
  - [concepts/物理演算](concepts/物理演算.md) — 新規セクション「現コードでの実装(2026-05 時点)」。O(N²) 構造とアルゴリズム選択の整合性を明記
  - [concepts/辺ラベル](concepts/辺ラベル.md) — 新規セクション「2026-05: コード上はデータモデルが既に完成している」。残課題を「フィールド追加」から「UX 動線」へ書き換え
  - [concepts/N項関係](concepts/N項関係.md) — 新規セクション「コード上の現状」。スキーマ `items: Array(RTItemId)` で N 項表現可、不足は UI 動線
  - [concepts/線を引く機能](concepts/線を引く機能.md) — 新規セクション「コード上の線種」。`heads` + `is_doubled` で全線種を表現、エッジクリック不可の正確な原因
  - [concepts/源の長文](concepts/源の長文.md) — Plan B 最小改造の 3 つのデータモデル案(`RTKozaneItem` 拡張 / 新 Item type / 別 collection)を追記
  - [entities/Kozaneba](entities/Kozaneba.md) — 新規セクション「実装上の事実」。スタック、Item 4 種、annotation のスキーマ、movidea 残存、隠しフラグを集約
  - [themes/Canvas移行の検討](themes/Canvas移行の検討.md) — 新規セクション「DOM 実装のボトルネック構造」。Canvas 化と並行して必要になる物理アルゴリズム / ヒットテスト / 状態管理スコープの 3 点を明示
- 効果:
  - 「辺ラベル機能が無い」「N項関係が実装されてない」という古い書きぶりを最新コードに合わせて修正
  - Plan B 期間に着手する改造の起点として「辺ラベル UX 動線」と「源の長文フィールド」が独立した最小 2 軸であることが各ページから到達可能に
  - Canvas 移行論が描画エンジン単独の話ではなく、物理 / ヒットテスト / state shape の 3 連動コストを評価する話だと明示

## [2026-06-01] query | 最新技術知識による実装改善の 3 方向(データ / 線UI / 畳むUI)

- nishio から「今の最新の技術知識で実装を改善したい」として 3 つの問題提起。本人の論点は既に wiki 化済みなので、ドメイン専門家相手の規範に従い、**外向きの角度**(外部ライブラリ・先行ツール・研究文献)を持ち込んで議論。
- 3 つの軸を並列でリサーチ:
  - **データ層**: Yjs / Automerge 3 / Loro / Replicache→Zero / Triplit / ElectricSQL / PowerSync / SQLite WASM + sqlite-vec / libSQL / Turso / PGlite / Iroh-blobs / Patchwork
  - **線 UI**: Miro / FigJam / tldraw / Excalidraw / Whimsical / Obsidian Canvas / Apple Freeform / Heptabase / Kinopio / Lucidchart / draw.io / yEd / OmniGraffle / Tana / Roam / Logseq / mermaid / D2 / Scapple
  - **畳む UI**: Pad++ / Bret Victor "Magic Ink" / tldraw frames / Figma frames + sections + auto-layout / FigJam sections / Miro frames / Heptabase section + nested wb / Muse / Kosmik / Obsidian Canvas / Apple Freeform / Workflowy / Tana / Magic Lens / Hierarchical Edge Bundling / Notion AI / Miro AI Mind Map
- 結論を 3 つの新規 theme ページに集約:
  - [themes/データモデル刷新の選択肢.md](themes/データモデル刷新の選択肢.md) — 「Loro doc + Firebase Storage の CAS」を Plan B 段階移行案、「Loro + SQLite WASM + 自前 CAS」を Plan A 書き直し案として提案
  - [themes/線UIサーベイ_2026.md](themes/線UIサーベイ_2026.md) — 「複数選択 → 線」は mainstream 不在で守るべき強み、追加で Miro 型ホバーハンドル + FigJam quick-create、辺ラベルは lazy edit、N項関係はジャンクション描画、Kinopio 型 connection-type を検討、と優先度順に整理
  - [themes/畳むUIの再設計.md](themes/畳むUIの再設計.md) — group を frame に格上げ(隙間問題の根本解)、Heptabase 型 inverse-zoom title で semantic zoom 実装、AI 自動表札(Notion AI 型)、Magic Lens で「開かずに覗く」、Hierarchical Edge Bundling で fold 横断のエッジを残す
- index.md に 3 ページを Themes セクションに追加。
- 効果:
  - 既存の「[源の長文](concepts/源の長文.md) / [線を引く機能](concepts/線を引く機能.md) / [なめらかな畳まれ](concepts/なめらかな畳まれ.md) / [活用されなかった機能](themes/活用されなかった機能.md)」が「何が問題か」までしか語っていなかったところに、**外部実装の比較と具体的な改修案** が追加された
  - Plan B / Plan A 双方の作業を駆動する判断材料として、データ層 / UI 層の両方で「次に着手すべき最小手数」が明示された
  - 3 ページが相互に「同時期に進めたサーベイ」として相互リンクしているので、片方を開けば他方に到達できる

## [2026-06-02] fill back | Loro ホスティング選択肢を [データモデル刷新の選択肢] に追記

- 前日のセッション末で残した open question 「Loro provider を誰が書くか」に対し、外部調査を実施。**Loro 公式マネージドサービスは 2026-06 時点で不存在**(Liveblocks / Hocuspocus / y-sweet 相当が無い)、エコシステム規模は Yjs と 2 桁差。
- 2025 後半の **[Loro Protocol](https://loro.dev/blog/loro-protocol)** 公開で、参照実装(Node SimpleServer / Rust + SQLite)とコミュニティ実装(`@loro-extended/repo`、iroh-loro、typeonce sync-engine-web)が揃ってきた段階。
- [themes/データモデル刷新の選択肢.md](themes/データモデル刷新の選択肢.md) に「Loro のホスティング選択肢(2026-06)」セクションを追記、3 ルートを整理:
  - **ルート A. Firestore + Loro binary**(snapshot を bytea で保存、差分を append-only サブコレクション): 移行コスト最小、Plan B 即着手可
  - **ルート B. Cloudflare Durable Objects + R2**(DO 1 個 = 場 1 個、PartyKit + Loro Protocol frame 転送 ~200-300 行): 将来性◎、Loro Protocol の機能を全部使える
  - **ルート C. 自前 Node/Rust + Postgres**(`@loro-extended/repo` + Hono + Supabase): 運用負荷中、Yjs の Hocuspocus + Postgres パターンの Loro 版
- 短期 A / 中期 B / 長期 C の判断を明示。Sources にホスティング関連 URL 9 件追加、index.md の要約を更新。
- 効果:
  - データモデル刷新ページが「Loro を採用するとどうやって動かすのか」まで答える状態になった
  - Plan B 第一歩(Firestore 残しで Loro 化)の実装規模が見える(provider 数百行)
  - Loro エコシステム未成熟というリスクを明示することで、Yjs を選ぶ判断材料も同時に提供

## [2026-06-02] lint | raw/a.txt → raw/関係UI議論_GPT.md にリネーム + CLAUDE.md に raw/ 命名規約追加

- nishio の指摘:プレースホルダ名 `raw/a.txt` のまま運用していたが、CLAUDE.md に raw/ ファイル命名規約が書かれていなかった。重要ソース(13 wiki ページ × 46 箇所から参照、関係場 / 関係を第一級にする / Clean Relation Elicitation の起源)であるほどファイル名から内容を読み取れるべき。
- 対応:
  - `git mv raw/a.txt raw/関係UI議論_GPT.md`
  - wiki/ 内の `raw/a.txt` を `raw/関係UI議論_GPT.md` に sed 一括置換(13 ファイル、リンク URL・bullet・display text すべて含む)
  - [CLAUDE.md](../CLAUDE.md) の「命名規約」セクションに raw/ 規約を追加:プレースホルダ名禁止、推奨形式 `raw/<内容>.md` または `raw/<YYYY-MM>_<内容>.md`、複数ラウンド対話なら代表テーマで命名、**ingest 工程の最初に命名する**(後でリネームすると参照更新コストが累積)、scrapbox_kozaneba/ などサブディレクトリは独自規約に従う
- 効果:
  - raw/ 直下の 2 ファイルが両方とも内容を表す名前に(`init.txt` / `関係UI議論_GPT.md`)
  - 今後の ingest で同じプレースホルダ運用が起きないようガードレール追加
  - 公開リポジトリで raw/ を眺めた人にも内容が伝わる

## [2026-06-02] query | より良くするための計画 → Plan B 試行(辺ラベル UI)に決定

- 「Kozaneba を改善するか / 新規プロダクトを作り直すか」の判断土台を整える進め方を相談。
- まず重い 5 段階ロードマップ(Phase 1: 判断フレーム化 → Phase 5: 分岐判断記録)を提示したが、nishio の判断:
  - **Plan A は別途。まず Plan B を一手だけ試す**
  - **仮説**: 賢い AI Agent は過去実装の悪いところを簡単に直す
  - 重い計画ではなく **1 回の実験で仮説検証** が本質
- 最初の題材を 4 候補(辺ラベル UI / 線UI / データモデル刷新 / 畳むUI)から選んでもらい、**辺ラベル UI** に決定。境界明確・スキーマ済み・実用価値高・失敗してもダメージ小。
- 新規ページ [Plan B 試行 2026-06](themes/Plan_B試行_2026-06.md) を作成:
  - AI Agent に渡せる仕様(必須要件 5 / アンチパターン 3 / 推奨パターン 3)
  - 既知の技術障害(`AnnotationLayer.tsx` の `pointerEvents: "none"` + `is_clickable` 切替)とその代替案 3 つ
  - 成功条件チェックリスト 7 項目
  - 観察ポイント(どこで詰まったか / 人間介入量 / 驚き / 仮説への感触)を試行後に記録する placeholder
- 効果:
  - wiki が「考えるだけ」から「実装に橋渡しする」モードに進む足場ができた
  - 試行結果を同じページに追記すれば、仮説検証の生データが wiki に蓄積される

## [2026-06-02] query | Plan B 試行仕様の曖昧点レビュー

- [themes/Plan_B試行_2026-06.md](themes/Plan_B試行_2026-06.md) を、[sources/kozaneba-code-architecture.md](sources/kozaneba-code-architecture.md) と最新化済み `work/kozaneba/` main に照らして確認。
- `work/kozaneba/` は `git fetch origin` / `git pull --ff-only` の結果 `Already up to date`。現 HEAD は `5de81c2`。
- 指摘候補: `line_start != null` は現コードでは不正確で、未設定値は `""`。また `LineAnnot.tsx` は `custom.is_clickable` を見ず `const is_clickable = false` 固定なので、仕様の技術前提を補足した方がよい。
- 追加で、表示本体は `Line.tsx` ではなく `LineAnnot.tsx`、保存には `mark_local_changed()` が必要、inline 編集 UI の確定/キャンセル/ショートカット干渉を明示した方が AI Agent の実装ぶれを減らせると整理。

## [2026-06-02] implement | Plan B 辺ラベル UI 初回実装

- 開始前に既存 wiki 変更を `bfcb16e Add Plan B edge label UI trial spec` として commit。
- `work/kozaneba/` に辺ラベル inline 編集を実装:
  - `LineAnnot.tsx` に透明 hit area を追加し、通常時の double-click で編集開始
  - `foreignObject` + `input` による線中央 inline editor
  - `Enter` / blur で確定、`Escape` でキャンセル、空文字は `delete annotation.label`
  - `mouseState === "making_line"` 中は hit area を出さず、線終端確定を邪魔しない
  - 確定時に `mark_local_changed()` を呼んで保存経路に乗せる
- 検証:
  - `npm test -- --watchAll=false`: pass
  - `npm run build`: pass
  - Cypress spec は追加したが、Cypress 12.4.0 binary が macOS 上で `bad option: --smoke-test` により起動不能。E2E は未実行
  - in-app Browser は `iab` が空で利用不可
- 追加対応:
  - 当初 `#blank` を試用 URL として案内したが、`#blank` は空の場なので白いキャンバスになる。確認しやすいよう `#tinysample` を 2 こざね + 1 線のサンプルに変更
- [themes/Plan_B試行_2026-06.md](themes/Plan_B試行_2026-06.md) の結果・含意セクションに初回実装の観察を追記。

## [2026-06-02] implement | #blank 表示と Cypress 実行環境の再確認

- nishio の指摘: `#blank` でも AppBar / StatusBar は表示されるべきで、完全な白画面なら UI が落ちている。Cypress 未実行のまま人間に確認要求するのも不適切。
- 再確認:
  - headless Chrome screenshot で `http://localhost:3000/#blank` の AppBar / DEV / Help / StatusBar が見えることを確認
  - Cypress spec に `#blank` の AppBar / canvas 表示確認を追加
  - Cypress 起動不能の原因は `ELECTRON_RUN_AS_NODE=1`。`env -u ELECTRON_RUN_AS_NODE` で外すと Cypress 12.4.0 は起動する
  - `env -u ELECTRON_RUN_AS_NODE npx cypress run --spec cypress/e2e/kozaneba/test_line_label.cy.ts`: 3 tests pass
- 修正:
  - `#tinysample` のサンプル変更は本質から外れるので `work/kozaneba` では revert
  - Plan B ページの検証結果を「Cypress 未実行」から「Cypress pass」に更新

## [2026-06-02] implement | #blank 白画面 boot 経路の修正

- nishio の再指摘を受け、`#blank` 完全白画面を「空の場」ではなく UI boot 失敗として扱い直した。
- 原因候補として、初期 HTML の gstatic Firebase UI CSS 同期 stylesheet が React bundle 実行前の白画面を作りうることを特定。
- `work/kozaneba` で追加修正:
  - `public/index.html` から Firebase UI CSS の外部 stylesheet を削除
  - JS 起動前も完全白画面にならない Kozaneba boot fallback を `#root` に追加
  - Firebase UI CSS は `SignDialog` / `CloudSaveDialog` を開いた時だけ動的ロード
  - `authui.start(...)` を render 中実行から `useEffect` に移動
- 検証:
  - `env -u ELECTRON_RUN_AS_NODE npx cypress run --spec cypress/e2e/kozaneba/test_line_label.cy.ts --config video=false`: 4 tests pass
  - `npm test -- --watchAll=false`: pass
  - `npm run build`: pass
  - `www.gstatic.com` 遮断の headless Chrome screenshot でも `#blank` の AppBar / StatusBar が表示されることを確認
- `work/kozaneba` commit: `7d36437 Prevent blank boot from external auth CSS`

## [2026-06-02] implement | 保存済み onLoad による boot 停止の防止

- 再考:
  - headless Chrome / Cypress は新規プロファイルで通る一方、人間の通常 Chrome だけ白画面になるなら、通常プロファイルに残る状態が第一候補
  - Kozaneba は起動時に `localStorage.onLoad` を `eval` しており、保存済みユーザースクリプトの例外が React mount 前に boot を止めうる
- `work/kozaneba` で追加修正:
  - `run_user_script()` を `try/catch` で保護し、失敗してもアプリ本体を起動し続ける
  - Cypress に壊れた `localStorage.onLoad` を注入しても `#blank` UI が表示されるテストを追加
- 検証:
  - `env -u ELECTRON_RUN_AS_NODE npx cypress run --spec cypress/e2e/kozaneba/test_line_label.cy.ts --config video=false`: 5 tests pass
  - `npm test -- --watchAll=false`: pass
  - `npm run build`: pass
- `work/kozaneba` commit: `93d9102 Keep booting when saved user script fails`

## [2026-06-02] implement | Codex 実装前 preflight の追加

- nishio の指摘: 機能実装に入る前に、Codex が正しく実装・検証できる環境を整える必要がある。現状は根本的におかしい。
- `work/kozaneba` に `scripts/codex-preflight.sh` と `npm run codex:preflight` を追加。
- preflight の役割:
  - dev server と `#blank` boot contract の確認
  - 初期 HTML が gstatic Firebase UI CSS に依存しないことの確認
  - dev server bundle が現在の実装を配っていることの確認
  - `ELECTRON_RUN_AS_NODE` を外した Cypress 実行
  - unit test と production build
- `npm run codex:preflight`: pass。
- 含意: 今後の Kozaneba 本体実装では、機能改修の前に preflight を通す。preflight が落ちたら環境整備を優先する。
- `work/kozaneba` commit: `898daf2 Add Codex implementation preflight`

## [2026-06-02] query | CI と Cypress ベースライン確認

- `work/kozaneba` には `.github/workflows/` がなく、GitHub Actions は GitHub 管理の `CodeQL` / `Dependabot Updates` のみ。PR #36 も Netlify deploy preview 系 check だけで、`npm test` / Cypress は走っていない。
- `netlify.toml` の build command は `CI=false npm install --legacy-peer-deps && npm run build` で、テストは実行しない。
- `origin/main` (`5de81c2`) を別 worktree + port 3001 で確認し、`env -u ELECTRON_RUN_AS_NODE npx cypress run --config baseUrl=http://localhost:3001,video=false` は `39 specs 中 20 specs failed`。
- ローカル main (`898daf2`) は追加した line label spec を含めて `40 specs 中 20 specs failed`。追加 spec は通っており、Cypress 失敗は既存ベースライン由来と判断。
- 含意: 新規機能より先に、CI 必須 subset / legacy・emulator 依存 spec の quarantine / UI テスト helper 整備を決める必要がある。

## [2026-06-02] query | Kozaneba テスト基盤調査の theme 化

- コード/CI/Cypress 調査で分かったことを [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に整理。
- 内容: GitHub Actions がアプリテストを走らせていない事実、Netlify が build のみで test しないこと、`origin/main` の Cypress 既存ベースライン失敗、失敗分類(Firebase emulator / AddKozaneDialog / pointer-events / pixel exact / legacy movidea)。
- Firebase emulator について、`firebase.json` には Auth 9099 / Firestore 8080 / Functions 5001 の設定があるが、調査時点では CLI と emulator 起動がないことを明記。
- 含意: Plan B の次工程は新機能追加ではなく、CI 必須 subset、emulator 付き integration test、legacy spec quarantine、UI test helper 整備を先に固める。

## [2026-06-02] implement | Firebase emulator smoke の追加

- `work/kozaneba` に `firebase-tools@12.9.1` を追加。最新 15 系は Node 20+ 要求なので、Netlify の Node 16 設定と衝突しにくい 12 系を選択。
- `scripts/cypress-emulator-smoke.sh` と `npm run cypress:emulator-smoke` を追加し、dev server 起動確認 + Auth/Firestore emulator + Cypress smoke を一括実行できるようにした。
- Cypress の Firebase import を `firebase/compat/app` / `firebase/compat/auth` に揃え、`toUseEmulator()` を Firestore だけでなく Auth emulator も接続する入口に変更。
- `AddKozaneDialog` の textarea を native textarea にし、小さい viewport でも DialogActions に覆われない高さ制約を追加。
- 検証:
  - `npm run cypress:emulator-smoke`: pass (`movidea/login`, `movidea/save`, `kozaneba/test_tutorial`)
  - `npm run codex:preflight`: pass
  - 全 Cypress with emulator: `40 specs 中 16 specs failed`, `55 tests 中 17 tests failed`
- 含意: Firebase emulator 不在で落ちていた auth/save/tutorial 系は CI smoke に載せられる状態になった。残りは主に legacy movidea の座標/pointer-events/メニュー操作前提。

## [2026-06-02] query | テスト基盤調査からの学びの file back

- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に「今回の学び」節を追記。
- 学びとして、`テストが通る` の主語を gate ごとに分けること、CI がなければ main の Cypress failure は自然に蓄積すること、Firebase emulator 導入は compat import / early auth emulator connection も含むことを整理。
- Cypress の actionability failure が実 UI の欠陥を示す場合があること、direct trigger と実ユーザー操作を混同しないこと、pixel exact assertion は最後の手段にすることを明文化。
- 次の一手として、全 Cypress ではなく `codex:preflight` と `cypress:emulator-smoke` を GitHub Actions に載せる方針を記録。

## [2026-06-02] query | Movidea 系テストを使っていたか

- [entities/Movidea](entities/Movidea.md) と [themes/系譜](themes/系譜.md) では、Movidea は Regroup をテスト可能に作り直した前身で、Cypress + React-N / Firebase Auth・Firestore のテストがあったと整理済み。
- 現行 `work/kozaneba` には `cypress/e2e/movidea/` spec が 23 本残っているが、`.github/workflows/` はなく、CI の必須 gate としては使われていない。
- `npm run cypress:emulator-smoke` は `movidea/login.cy.ts` と `movidea/save.cy.ts` を含むため、Movidea 系の一部は smoke test として再利用されている。
- ただし [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) の通り、全件は legacy 座標期待値 / pointer-events / 旧 UI 前提でまだ落ちるため、現行品質 gate とは分けて扱う必要がある。

## [2026-06-02] query | Movidea 以外の Cypress は通っているか

- `env -u ELECTRON_RUN_AS_NODE npx firebase emulators:exec --only auth,firestore "env -u ELECTRON_RUN_AS_NODE npx cypress run --spec 'cypress/e2e/kozaneba/*.cy.ts' --config baseUrl=http://localhost:3000,video=false"` を実行。
- 結果は `17 specs 中 1 spec failed`, `30 tests 中 1 test failed`。
- 落ちたのは `cypress/e2e/kozaneba/test_drag.cy.ts` の `bug fix: drag out from nested groups(drag G1 in/out)` だけ。`cy.testid("1").should("hasPosition", [225, 225])` で、期待 x=225 に対して実測 x=25。
- したがって Movidea 以外も全通ではないが、失敗は既知分類の nested drag 座標期待値 1 件に限定される。

## [2026-06-02] query | test_drag failure は前から落ちているか

- `origin/main` (`5de81c2`) を別 worktree + port 3001 で起動し、`cypress/e2e/kozaneba/test_drag.cy.ts` 単体を実行。
- 結果は現行 HEAD と同じく `7 tests 中 1 failed`。失敗も同じ `bug fix: drag out from nested groups(drag G1 in/out)` で、期待 x=225 に対して実測 x=25。
- `git blame` では該当 test は 2021-12-27 の `refactor tests` 由来。2023-01-27 の `ignore some tests` では隣接する `drag G2` と `closed group in another group` がコメントアウトされたが、この `drag G1 in/out` は残されていた。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に、これは少なくとも PR #36 merge 時点で既に落ちており、基本的な nested drag regression として扱うべきだと追記。

## [2026-06-02] query | test_drag failure の bisect

- `891f36d` (`ignore some tests`) は `cypress/e2e/kozaneba/test_drag.cy.ts` 7 tests 全通。`origin/main` (`5de81c2`) は `drag G1 in/out` で同じ `expected x:25 is 225` failure。
- 通常の checkout + lockfile 固定で最初に再現可能な bad commit は `ceea496` (`依存パッケージの更新: yarn.lockとpackage-lock.jsonを追加`)。この commit 自体は lockfile のみ。
- `6f440f4` (`React 17→18、Firebase 8→9`) から `3785e91` までは package.json / yarn.lock 不整合で frozen install できず skip。ただし `6f440f4` を別 worktree で non-frozen install すると同じ target failure を再現。
- 結論: 実質的な退行は `6f440f4` の大規模依存更新に入った。React 18 / `createRoot` / styled-components 6 / MUI 更新のどれが直接原因かは未分離。

## [2026-06-02] ingest | キャンバス状態テスト戦略

- チャットで共有された「より良いテスト」メモを [raw/2026-06-02_キャンバス状態テスト戦略.md](../raw/2026-06-02_キャンバス状態テスト戦略.md) として保存。
- [sources/canvas-state-testing-strategy-2026-06.md](sources/canvas-state-testing-strategy-2026-06.md) を作成し、状態モデル、world/screen 座標変換、イベント列、E2E、visual regression の役割分担を要約。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に、pixel exact assertion の代替方針として「world 座標・状態モデルを主テスト対象にする」節を追記。
- 現行 Kozaneba の `window.movidea` / `cy.getGlobal()` は test API 的な入口として使えるが、`world_to_screen` と `zoom_around_pointer` は pure helper 化して unit test しやすくする余地があると整理。

## [2026-06-02] query | Cypress / Playwright / Webwright と単体テスト化

- Kozaneba では、短期的には既存 Cypress 資産を捨てず、smoke / regression の薄い E2E gate として整理するのが妥当。
- Playwright は clean-slate browser context、auto-waiting、parallel 実行が強く、将来の少数の本物 E2E や新規プロダクトでは第一候補になりうる。
- Webwright は Playwright を使う AI browser agent framework であり、CI の決定的テスト基盤ではなく、探索・テスト生成・調査補助として扱うのがよい。
- 重要なのはツール選定より、座標変換・drag/drop・selection・nested group などを world 座標 / state invariant の unit / integration test に寄せ、E2E は代表操作だけにすること。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に「Cypress / Playwright / Webwright の使い分け」として file back。

## [2026-06-02] query | 公開版の nested drag を人間がテストすべきか

- `https://kozaneba.netlify.app/#blank` は HTTP 200 を返すことを確認。
- 既存 `test_drag.cy.ts` を公開 URL に向けると、production build には `window.movidea` がないため全 7 cases が hook 不在で失敗し、drag 挙動の判定にはならなかった。
- `window.kozaneba` / `try_to_import_json` と UI 操作を使った一時 Cypress check も試したが、既知 bad のローカル main でも通ったため、`drag G1 in/out` regression の検出器としては不採用。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に、公開版確認は自動テストだけでは断定できず、人間が nested group drag をピンポイントで実 UI 確認するのが最短だと追記。

## [2026-06-02] query | A(B(C)) nested drag の人間観察

- nishio から、`A(B(C))` を作って `C` を root に出した後、`A` の sibling としては自然だが `B` の sibling にした時に位置が不自然になる、という実 UI 観察を受けた。
- `drag_drop_item.ts` と `drag_drop_item_into_group.ts` を確認し、root への drop と group への drop で親 offset の扱いが分かれていることを確認。
- `get_total_offset_of_parents.ts` は draft state `g` を受け取るが、親探索で `find_parent(current_parent)` に state を渡していないため、更新中の親子関係と現在 state が混ざる疑いがある。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に、人間観察と次の優先調査点として追記。

## [2026-06-02] query | nested drag offset 修正

- dirty な `work/kozaneba` は触らず、detached clean worktree `work/kozaneba-drag-investigation` を使って修正。
- `drag_drop_item_into_group.ts` の root → nested group branch で、drop 先 group 単体の `position` ではなく親 chain 全体の offset を引くように変更。
- `get_total_offset_of_parents.ts` では `find_parent(current_parent, g)` を使い、渡された state と親探索をそろえた。
- `test_drag.cy.ts` に offset 付き `A(B(C))` 相当の regression を追加し、既存 `drag G1 in/out` は React 18 の描画安定後に次 drag を始めるよう調整。
- 検証: `npm test -- --watchAll=false` pass、`npm run build` pass、`test_drag.cy.ts` は 8 tests pass、Firebase emulator 付き `cypress/e2e/kozaneba/*.cy.ts` は 17 specs / 31 tests pass。
- commit `5b19a29` を `fix/nested-drag-offset` として push し、draft PR [nishio/kozaneba#39](https://github.com/nishio/kozaneba/pull/39) を作成。

## [2026-06-02] query | react-scripts 由来の Dependabot 残課題を Wiki に書いたか

- まだ記録していなかったため、[themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に「Security alert 修正と CRA 移行の必要性」節を追記。
- Code scanning は `get_scrapbox_page` の固定 origin 化、旧 proxy 停止、Scrapbox URL 判定強化、複数改行 sanitize 修正により open alert 0 件になったと整理。
- Dependabot は `react-scripts > webpack-dev-server` 由来の medium 6 件だけが残り、`webpack-dev-server@5.2.4` の単純 override は CRA の dev server 設定と互換性がなく `npm start` を壊すため不採用だと記録。
- 完全解消には CRA / `react-scripts` からの移行、または `react-scripts` の eject / fork / patch が必要で、短期的には dev server を外部公開しない運用でリスクを限定する開発基盤負債として扱う、と明文化。

## [2026-06-02] query | テスト改善計画のページを執筆

- [themes/テスト改善計画.md](themes/テスト改善計画.md) を新規作成。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) の調査結果を、Phase 0〜6 の実行計画として再構成。
- 方針は、全 Cypress 緑化や Playwright 全面移行ではなく、state / world 座標の unit・integration test を厚くし、Cypress は薄い smoke / regression gate に整理すること。
- P0/P1/P2 優先順位、成功条件、今やらないこと、CI 化、Playwright 少数導入、visual regression の限定利用を明記。

## [2026-06-02] query | CI ではなるべく多くのテストを動かすべき

- nishio の指摘: 「全 Cypress はまだ必須 gate ではない」が、CI で動かさないように読める。なるべく多くのものを CI で動かすべきではないか。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) を修正し、CI で実行する範囲と merge を止める required gate を分離。
- Required CI / Non-required CI / Scheduled CI / Quarantine CI の層を追加。
- 方針: full / legacy / quarantine test も CI 上で観測し、artifact と失敗件数を残す。ただし既知 failure は最初から required check にしない。

## [2026-06-02] query | Movidea 以外のテストは全部通るのでは

- nishio の指摘: nested drag 修正後は Movidea 以外の Kozaneba Cypress は全部通るようになったのではないか。
- `work/kozaneba` の main は `8d14d5b` で、nested drag 修正 `5b19a29` はまだ `fix/nested-drag-offset` / `work/kozaneba-drag-investigation` 側。
- `work/kozaneba-drag-investigation` で `cypress/e2e/kozaneba/*.cy.ts` を Firebase emulator 付きで再実行し、`17 specs / 31 tests` 全 pass を確認。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) を修正し、`fix/nested-drag-offset` が main に入った後は `cypress:kozaneba-all` を required CI に載せる方針へ変更。

## [2026-06-02] query | PR #39 merge 状態の再確認

- nishio の指摘どおり、`origin/main` は `c46b5fc` で PR #39 merge 済み。`5b19a29` は `origin/main` に含まれている。
- `work/kozaneba` も `main@c46b5fc` になっていることを確認。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) の条件付き表現を修正し、PR #39 merge 後の `origin/main` / `work/kozaneba` では Kozaneba Cypress 17 specs / 31 tests が通る、という現状に更新。

## [2026-06-02] query | Node 更新と CRA 脱出はテスト改善後

- nishio の方針: Node が古い問題と CRA をやめたい問題は、テストがいい感じになった後で移行したい。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) に Phase 7「Node 更新と CRA 脱出はテスト gate 安定後に行う」を追加。
- 移行開始条件として、Required CI 安定、`cypress:kozaneba-all` required 化、full / legacy / quarantine の CI 観測、主要座標ロジックの unit / integration test 化、移行前 artifact 保存を明記。
- P2 から CRA / Vite 移行を外し、P3 として Node version 更新、CRA / Vite migration spike、Dependabot 残 alert 解消に移した。

## [2026-06-02] query | テスト改善計画への追加指摘

- nishio の指摘:
  - Movidea legacy はそもそも通るべきテストなのか分からない。
  - full Cypress に何が残っているのか不明。
  - 座標ロジック単体テスト化には nested drag 修正の知見を入れるべき。
  - Cypress helper 整理はよいが、順番は先すぎるかもしれない。
  - Playwright は必要性がないなら入れず、Cypress だけでよいなら複雑にしない方がよい。
- `work/kozaneba` current main には Kozaneba 17 specs と Movidea 23 specs がある。Kozaneba 17 specs / 31 tests は pass 済み。
- Movidea 23 specs を emulator なしで実測し、17 specs failed / 7 tests pass / 18 tests failed。主な失敗は旧 DOM/class/text 前提、pixel exact 座標期待、Cypress actionability、Firebase emulator なし、旧 API 前提。
- `firebase-tools@15` は Java 21 未満を拒否するため、emulator 付き full run には CI runner の Java 21 固定が必要。Cypress 13.17.0 も binary cache / install が必要。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) を修正:
  - Movidea は「通すべきか分類する対象」とし、一括で直す対象から外した。
  - nested drag 修正の知見(`find_parent(current_parent, g)`、parent chain offset、`A(B(C))`)を Phase 2 に明記。
  - Cypress helper 整理は、残す spec を分類した後に行う順序へ変更。
  - Playwright は concrete gap が出るまで入れない方針へ変更。

## [2026-06-02] query | nested drag 修正アプローチの file back

- nishio から、PR #39 merge 後に deploy 環境で実 UI 動作確認し、今回のアプローチでうまく直せたと共有された。
- [themes/Kozanebaテスト基盤調査_2026-06.md](themes/Kozanebaテスト基盤調査_2026-06.md) に、PR #39 merge / deploy 確認済みの結果を追記。
- 同ページに「今回うまくいったアプローチ」として、clean worktree、実 UI の人間観察、Cypress failure の分解、parent chain / state 不変条件への変換、Kozaneba 系 Cypress 全体確認、deploy 確認の流れを整理。
- 学び: キャンバス UI の regression では、pixel exact assertion を症状検出器として使い、原因特定と恒久テストは world/state 側に寄せる。

## [2026-06-02] query | nested drag ソースコード読解の知見

- nested drag 修正時に読んだソースから、Kozaneba の位置は root 座標と親グループ相対座標が混在し、絶対位置は親グループ鎖の offset 合計で決まることを整理。
- drag/drop は canvas drop と group drop で別ルートになり、root↔nested / nested↔nested の座標変換と `normalize_group_position` がバグりやすい中心であると確認。
- Cypress の `do_drag` は直接 DOM event を発火するため、人間操作とは異なる render timing / stale DOM の false negative があり、React 18 以降は明示的な待機や状態確認が必要だと整理。

## [2026-06-02] query | テスト改善計画の順序を3段階に整理

- nishio の整理: 1 座標ロジック単体テストに今回の知見を入れる、2 Kozaneba 本体側を required CI にする、3 CI gate 安定後に Node / CRA 脱出。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) をこの3段階に再構成。
- Movidea legacy は通すべきか棚卸し対象、Playwright は concrete gap が出るまで入れない、Cypress helper 整理は残す spec を決めた後に変更。

## [2026-06-03] query | GitHub Issue 全文読解からの知見を Wiki 化

- `nishio/kozaneba` の AI 生成 open Issue 20 件を全文読解し、5 件だけ残して 15 件を理由コメント付きで close した判断を [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md) に保存。
- 残した Issue は #4 console log、#12 browser extension docs、#13 CI/CD、#17 test coverage、#25 performance profiling。いずれも現在のコード事実に接続し、次の一手を小さく切れる。
- 閉じた Issue は、古い前提、性能系の手段先行、UX/i18n/accessibility の一般論、大規模リファクタ単独 Issue として分類。
- 学び: AI が生成した Issue は実行キューではなく提案の束であり、全文読解・コード照合・計測前提の分離・再発行条件の明示を経て Plan B の backlog に変換する必要がある。

## [2026-06-03] query | Sentry の運用エラーも観察するべき

- 現行コードを確認し、`work/kozaneba/src/initSentry.tsx` で production 時に Sentry が初期化され、例外時 report dialog と `getGlobal()` context 設定があることを確認。
- [entities/Sentry.md](entities/Sentry.md) を新規作成し、2021-08 の導入経緯、現行実装、ユーザ試行錯誤エラーを Sentry に送るべきでないという注意点を整理。
- [themes/運用エラー観察.md](themes/運用エラー観察.md) を新規作成し、Sentry issue を本体バグ、入力 validation、UserScript / 拡張、外部サービス、ブラウザ依存、開発ノイズに分類して読む手順を定義。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) と [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md) に、Sentry を production 実害ベースの観測ソースとして接続した。

## [2026-06-03] query | CI安定化とVite移行の実施記録

- [themes/CI安定化とVite移行_2026-06.md](themes/CI安定化とVite移行_2026-06.md) を新規作成し、PR #40〜#44 による座標ロジック単体テスト、required CI、Node 24、CRA から Vite への移行、Vite 環境 API 後処理を記録。
- 学びとして、CI gate を先に安定させると移行 failure を切り分けやすいこと、Firebase emulator は GitHub CI で Java 21 固定により動くこと、required CI と観測 CI は分けるべきことを整理。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) に、3段階計画が完了済みであることと、今後は #17 を小さな regression に分解する方針を追記。
- [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md) に、#13 CI/CD パイプライン構築は完了候補になったことを追記。次の実装候補は #13 の完了処理後、#4 production console log cleanup。

## [2026-06-03] query | direct eval warning は仕様変更にしない

- nishio の判断として、Vite build で見えた direct `eval` warning は [concepts/UserScript.md](concepts/UserScript.md) の仕様変更として扱わないことを記録。
- [themes/CI安定化とVite移行_2026-06.md](themes/CI安定化とVite移行_2026-06.md) の学びと次アクションを更新し、#13 完了処理、#4 production console log cleanup、direct `eval` warning の小さな後続作業という順序に整理。
- [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md) に、direct `eval` warning は既存の intentional な UserScript 実行を維持したまま明示・整理する後続作業だと追記。

## [2026-06-03] query | #17 と旧 Movidea テスト棚卸し

- nishio の指摘: #17「テストカバレッジの向上」は、座標変換・hit test・undo/redo・保存復元などの小さな regression に分解し、その際に旧 Movidea のテスト資産も棚卸しするべき。
- `work/kozaneba/cypress/e2e/movidea/` は 23 specs / 1452 lines。座標 / drag / group、selection / hit test、dialog / tutorial、import / save / restore / auth、価値が薄い・コメント化済み、に分類する方針へ整理。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) に、各 Movidea spec を promote / keep-quarantined / delete / rewrite-before-decision に分ける判断軸を追記。
- [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md)、[themes/CI安定化とVite移行_2026-06.md](themes/CI安定化とVite移行_2026-06.md)、[wiki/index.md](index.md) の #17 説明も同じ方針に更新。

## [2026-06-03] query | #13 と #4 の完了

- Issue #13 は、PR #41〜#44 と main CI success を根拠に完了コメントを付けて close。
- Issue #4 は、PR #45 `Route production console logs through dev logger` で runtime `console.log` / `console.time` / `console.timeEnd` を `dev_log` / `dev_time` に寄せ、merge により close。
- PR #45 は local の `npm test`、`npm run build`、`npm run codex:preflight`、`npm audit --audit-level=moderate`、PR CI、main post-merge CI が success。
- [themes/CI安定化とVite移行_2026-06.md](themes/CI安定化とVite移行_2026-06.md) と [themes/AI生成Issueのトリアージ.md](themes/AI生成Issueのトリアージ.md) を更新し、残る open Issue は #12 / #17 / #25 の 3 件だと整理。

## [2026-06-03] query | Movidea legacy test actual behavior audit

- Java 21 を Homebrew で導入し、`firebase-tools@15` の emulator 要件を満たした上で Movidea legacy Cypress を実測。
- `npm run cypress:emulator-smoke` は `movidea/login.cy.ts`、`movidea/save.cy.ts`、`kozaneba/test_tutorial.cy.ts` が pass。
- `cypress/e2e/movidea/*.cy.ts` 全体は 23 specs / 25 tests のうち 9 passing / 16 failing、Cypress summary では 15 of 23 specs failed。
- [sources/movidea-legacy-test-inventory-2026-06.md](sources/movidea-legacy-test-inventory-2026-06.md) を新規作成し、各 spec を promote 13 / rewrite-before-decision 5 / delete 5 に分類。
- [themes/テスト改善計画.md](themes/テスト改善計画.md) と [index.md](index.md) を更新し、#17 の分解方針を実測済みの Movidea 棚卸しへ接続した。

## [2026-06-03] query | 線を引く機能の実装箇所確認

- `work/kozaneba` は main / origin/main と一致し、コード本体は clean。未追跡は過去 Cypress failure の `cypress/screenshots/` のみ。
- 線は `src/Global/TAnnotation.ts` の `line` annotation として表現され、作成 UI は `src/Menu/AddLineMenuItem.tsx`、確定処理は `src/Event/handle_making_line.ts`、描画とラベル編集は `src/Canvas/Annotation/LineAnnot.tsx` にある。
- inline 辺ラベル編集は commit `9e7122b Add inline line label editing` で入り、その後 `7d36437` / `93d9102` / `898daf2` で白画面・保存済み user script・preflight が整備された。
- 再開時の確認入口は `npm run codex:preflight` と `cypress/e2e/kozaneba/test_line_label.cy.ts`。

## [2026-06-03] query | 未マージブランチ確認

- `git fetch --prune origin` 後に確認したところ、local branch はすべて `main` に merge 済み。
- remote で `origin/main` に未マージなのは `origin/devin/1741687705-update-dependencies` のみ。Firebase / React / dependency migration 系の古い分岐で、線ラベル実装とは別。
- 線ラベル系の `origin/devin/1757526061-line-labels` は merge 済み側にあり、main には PR #36 の Devin 実装と、その後の `9e7122b Add inline line label editing` が入っている。

## [2026-06-03] query | iPad で動かない原因候補と実機テスト手順

- `work/kozaneba` に未追跡 `cypress/screenshots/` があったため、`origin/main` から detached worktree `work/kozaneba-ipad-audit` (`d5757f5`) を作ってコードを確認。
- 現行キャンバス操作は `onMouseDown` / `onMouseMove` / `onMouseUp` 中心で、`touch_support` は定義のみ未使用。`get_client_pos` も `clientX/clientY` 前提で TouchEvent を扱わない。
- iPad 不具合の主候補として、mouse event 前提、group drop の hover 依存、`touch-action` 未指定、FirebaseUI の popup sign-in 固定を整理。
- [themes/iPad実機対応調査_2026-06.md](themes/iPad実機対応調査_2026-06.md) を新規作成し、PointerEvent 中心の修正方針、LAN dev server 接続手順、iPad 実機 smoke test checklist、Safari Web Inspector / Sentry での観測方針を保存。
- [index.md](index.md) に同ページを追加。

## [2026-06-03] query | iPad 実機自動テストはどの程度可能か

- Apple Safari WebDriver、Appium XCUITest / mobile web、Cypress、Playwright、BrowserStack の公式情報を確認。
- 結論: iPad 実機 Safari 自動化は可能だが、Cypress の延長ではなく、Selenium/safaridriver、Appium、または BrowserStack 等のクラウド実機サービスが必要。
- Cypress は desktop CI と viewport / WebKit 近似に留め、Playwright emulation は cheap regression、実機 smoke はまず手動、必要になったら Appium/Selenium または BrowserStack Playwright で狭く自動化する方針。
- [themes/iPad実機対応調査_2026-06.md](themes/iPad実機対応調査_2026-06.md) に「iPad実機自動テストの可能範囲」と「現実的な段階」を追記。

## [2026-06-03] query | 古いレンダリング方式を別デプロイで保持できるか

- [sources/kozaneba-code-architecture.md](sources/kozaneba-code-architecture.md) と [themes/Canvas移行の検討.md](themes/Canvas移行の検討.md) を読み、現行 Kozaneba は Netlify hosting / hash routing / Firestore `ba` collection の SPA であることを確認。
- `work/kozaneba` は dirty なので読むだけにし、`netlify.toml`、`vite.config.ts`、`src/App/App.tsx`、`src/Cloud/*`、`firestore.rules` で、同じ `#view=<ba>` を別ビルドから読む構成が自然に成立することを確認。
- Netlify 公式 docs で branch deploy / deploy permalink / rollback / locked deploy が現在も提供されていることを確認。ただし長期保存用途では単発 permalink より専用 branch または専用 site のほうが管理しやすい。
- 結論: 古い作品を壊さず見せる目的なら「古い viewer を read-only 別デプロイとして保持」は現実的。注意点はデータ schema migration、Firebase Auth authorized domain、古い依存の build 再現性、同じ Firestore への write を避けること。

## [2026-06-03] query | データ込み静的 HTML ダウンロードは可能か

- [sources/kozaneba-code-architecture.md](sources/kozaneba-code-architecture.md) と [themes/データモデル刷新の選択肢.md](themes/データモデル刷新の選択肢.md) を入口に、現行の「場 = JSON doc」構造を確認。
- `work/kozaneba` は dirty なので読むだけにし、`state_to_docdate` / `docdate_to_state` / `Blank` / `LocalBackup` / `copy_json` / 画像系 component を確認。Firestore doc 相当 JSON を HTML に埋めて起動時に state へ流し込む実装は小さく作れる。
- ただし完全な単一ファイル・オフライン閲覧を目指す場合、Firebase/Sentry/GA を含まない static viewer entry、inline bundle、JSON の script 埋め込み時 escaping、Gyazo/Scrapbox/favicons など外部画像の扱いを別途設計する必要がある。
- 結論: 可能。最初は「データ埋め込み + 同梱 read-only viewer + 外部画像は URL のまま」が現実的で、厳密なアーカイブは画像も data URI / ZIP へ取り込む後続段階に分ける。

## [2026-06-03] query | Movidea legacy test cleanup implementation

- `work/kozaneba` で旧 Movidea spec の最初の整理 pass を実施。
- `add_kozane_dialog`、login/save smoke、scale 表示、selection hit test、Regroup import、font-size algorithm、legacy `piece` upgrade を Kozaneba Cypress または unit test に移した。
- 旧 Movidea folder から、移植済み 6 specs と delete 判定 5 specs を削除し、残りは 12 specs に縮小。
- `scripts/cypress-emulator-smoke.sh` は Movidea path ではなく Kozaneba path の login/save/tutorial を実行するよう更新。
- 検証: `npm test` 8 files / 14 tests pass、`npm run build` pass、`npm run cypress:emulator-smoke` 3 specs / 3 tests pass、`npm run cypress:kozaneba-all` 21 specs / 40 tests pass。
- [sources/movidea-legacy-test-inventory-2026-06.md](sources/movidea-legacy-test-inventory-2026-06.md) に実装反映状況を追記。

## [2026-06-03] query | Movidea legacy test cleanup completion

- 残っていた Movidea 12 specs を、geometry / drag-drop state / selection make-group / font-size render contract へ分解して現行 test に移した。
- `src/dimension/item_layout.test.ts` と `src/Event/drag_drop_state.test.ts` を追加し、旧 pixel exact DOM assertion と direct HTML5 drag/drop の代わりに world/state invariant を固定。
- `cypress/e2e/kozaneba/test_adjust_font_size.cy.ts` を追加し、`test_selection.cy.ts` に selection -> make group regression を追加。
- `cypress/e2e/movidea/` の legacy specs は 0 本になった。
- 検証: `npm test` 10 files / 23 tests pass、`npm run build` pass、`npm run cypress:kozaneba-all` 22 specs / 42 tests pass。smoke は同じ Kozaneba login/save/tutorial path で pass 済み。
- [sources/movidea-legacy-test-inventory-2026-06.md](sources/movidea-legacy-test-inventory-2026-06.md) を最終状態へ更新。

## [2026-06-03] query | ブラウザの「完全保存」に外部画像アーカイブを任せられるか

- 静的 HTML を開いた後にブラウザの「Webpage, Complete」で保存してもらう案を検討。
- 結論: 手軽な fallback としては使えるが、長期アーカイブ仕様としては弱い。保存対象、外部画像、lazy load、module chunk、CSS/フォント、URL 書き換え、認証付き/リダイレクト付き画像の扱いがブラウザ依存になる。
- Kozaneba では「アプリ生成の静的 viewer + Ba JSON」を一次成果物にし、外部画像は最初 URL のまま。完全アーカイブが必要になったら app 側で画像 URL を列挙して fetch/data URI 化、または HTML + assets の zip 出力に進むのがよい。

## [2026-06-03] query | 静的 HTML ダウンロード MVP 実装

- `work/kozaneba` が dirty だったため、`work/kozaneba-static-html-export` detached worktree を作成して実装を隔離。
- `src/StaticExport/build_static_html.ts` を追加し、Firestore doc 相当 JSON を HTML 内の application/json script に埋め、軽量 read-only viewer でこざね / open・closed group / Scrapbox / Gyazo / line label / pan・zoom・fit を表示する MVP を実装。外部画像は URL のまま。
- `src/StaticExport/download_static_html.ts` と Main menu の `Download Static HTML` を追加し、現在の Ba を `state_to_docdate` で JSON 化して `.html` として Blob download する導線を追加。
- テスト: `npm test -- src/StaticExport/build_static_html.test.ts`、`npm test`、`npm run build`、`npm run codex:preflight` が pass。in-app browser ではメニュー表示を確認、download event は Codex in-app browser 非対応のため未検証。
- `codex/static-html-export` branch に commit `1e3d09e Add static HTML export` を作成し、draft PR [#47](https://github.com/nishio/kozaneba/pull/47) を作成。PR 作成直後の GitHub Actions は running。
- [sources/static-html-export-mvp-2026-06.md](sources/static-html-export-mvp-2026-06.md) を新規作成し、MVP の範囲、軽量 viewer にした理由、外部画像を URL のままにした判断、検証結果、今後の判断点を保存。

## [2026-06-03] query | iPad PointerEvent 対応実装

- `work/kozaneba` が dirty だったため、clean worktree `work/kozaneba-pointer-events` に branch `codex/ipad-pointer-events` を作成して実装を隔離。
- mouse / pointer / touch の座標正規化、canvas の pointer capture、`touch-action: none`、pointerup 座標ベースの group drop 判定を追加。
- `cypress/e2e/kozaneba/test_pointer_events.cy.ts` を追加し、synthetic `pointerType: "touch"` で drag、selection、group drop を確認。
- 検証: `npm run build`、`npm test`、`npm run codex:preflight` が pass。JDK 21 を `JAVA_HOME=/opt/homebrew/opt/openjdk@21` で指定し、`CYPRESS_BASE_URL=http://localhost:3001 npm run cypress:kozaneba-all` が 18 specs / 34 tests pass。
- `codex/ipad-pointer-events` branch に commit `e517035 Add pointer event input handling` を作成し、draft PR [#46](https://github.com/nishio/kozaneba/pull/46) を作成。
- [themes/iPad実機対応調査_2026-06.md](themes/iPad実機対応調査_2026-06.md) に実装メモ、PR 情報、未完了 gate としての iPad 実機 smoke test を追記。

## [2026-06-03] ingest | kozaneba-forum Release Notes(初期 ingest、JP forum 名称の誤りを含む)

- Cosense CLI (`cosense browsePage`) で https://scrapbox.io/kozaneba-forum/Release_Notes を取得し保存(449 行、2021-06-25 〜 2025-09-11)。
- **重要な誤り**: この時点で「日本語版 `kozaneba-forum-ja` は存在しない(HTTP 404)」と書いたが、正しい名前は `-jp` だった。次のエントリで修正。
- 新規ページ [sources/release-notes-2021-2025.md](sources/release-notes-2021-2025.md) を作成し、entities/Kozaneba.md / concepts/線を引く機能.md / concepts/Scrapboxこざね.md / themes/活用されなかった機能.md を更新。
- これらの更新内容は次の bilingual ingest で大幅に拡張・上書きされる。

## [2026-06-03] ingest | kozaneba-forum + kozaneba-forum-jp 全件 ingest

- 上の誤りの指摘を受けて再調査:日本語 forum は `kozaneba-forum-jp` で存在(`-ja` ではなく `-jp`)。[2021-08-20__Kozaneba開発日記2021-08-20.md](../raw/scrapbox_kozaneba/2021-08-20__Kozaneba開発日記2021-08-20.md) などで `/kozaneba-forum-jp/` の表記を確認。
- nishio の要望「多分どっちのフォーラムも全部読んでも大した量ではないと思うのでやって」に従い、両 forum の全ページを Cosense CLI で取得:
  - `raw/kozaneba-forum/` 17 ページ(EN)
  - `raw/kozaneba-forum-jp/` 48 ページ(JP)
- 既存の `raw/Release_Notes_kozaneba-forum.md` と `raw/Release_Notes_kozaneba-forum-jp.md` をそれぞれ subdir に移動・リネーム(`Release_Notes.md` / `リリースノート.md`)。
- wiki の参照パスを subdir 構造に一括更新(5 ファイル: entities/Kozaneba.md / concepts/線を引く機能.md / concepts/Scrapboxこざね.md / themes/活用されなかった機能.md / sources/release-notes-2021-2025.md)。
- [sources/release-notes-2021-2025.md](sources/release-notes-2021-2025.md) を全面書き直し:
  - **JP forum 不存在の誤り**を修正
  - EN/JP リリースノートの差分を明示:**JP のみ**に 2022-05-31(日本語チュートリアル自動化)/ 2025-04-27(PR #18-21, #29 の外部ユーザ報告 bug fix 集中リリース)/ 2025-08-09(Gyazo リダイレクト由来 menu 画像破損修正、PR #32) が記載
  - **「2 年半の沈黙」**主張を修正:2023-03 〜 2025-03 は実質沈黙だが、その後 2025-04-27 / 08-09 / 09-11 と段階的に再開
  - 外部ユーザ一覧(Foam_Crab, uchan_nos, k937gy, kusanagi, sta, YJ, reira, hoshihara)を整理
  - 公開された 3 つの設計原則を抽出:**「default で線が増えない方を選ぶ」**(Remove Split-Kozane 2023-02-27)、**「線をクリック可能にするとドラッグが妨害される」**(Scrapbox Integration 2022-05-26)、**「線は本質ではない、迷うなら近接で」**(Drawing lines 2022-03-08)
- 既存ページに新証拠を追記:
  - [entities/Kozaneba.md](entities/Kozaneba.md) — タイムラインの誤りを修正、外部ユーザ活動セクションを追加
  - [concepts/線を引く機能.md](concepts/線を引く機能.md) — エッジ pointerEvents:none の設計意図(Scrapbox Integration から)、外部ユーザ sta の Add Lines 混乱、「default で線が増えない方を選ぶ」原則を追記
  - [concepts/Scrapboxこざね.md](concepts/Scrapboxこざね.md) — リンクをクリック可能にしない設計判断、外部ユーザ YJ の crash 報告を追記
  - [themes/活用されなかった機能.md](themes/活用されなかった機能.md) — 「default で線が増えない方を選ぶ」原則を Split 削除事例の subsection として追加
  - [concepts/チュートリアル.md](concepts/チュートリアル.md) — 多言語化タイムライン(2021-08 英語版 → 2021-08 手動和訳 → 2022-05 改訂 → 2022-05-31 ブラウザ言語自動切替)、ユーザテスト募集(2021-08-19)を追記
  - [index.md](index.md) — sources/release-notes-2021-2025.md の説明文を「両 forum 全件」に更新
- 主要な再発見と確定事項:
  - 物理演算は 2021-09-16 以降リリースノートに登場せず default 機能化されていない事実は変わらず。**nishio 本人が「今は重視していない」と確認済**(feedback memory に保存)
  - 2025-09-11 のリリースで線ラベル機能は **user-facing 告知ゼロ**(コード merge 済み `4c9b38d` / `5de81c2` がリリースノート両 forum に非掲載)。**nishio 本人が「現時点でも動線がよくわからん」と確認**(feedback memory に保存)
  - JP forum は **ベータリリース時点(2021-08-20)から開設**、日英バイリンガル運営は最初から計画されていた。中国語フォーラムは未着手
  - EN/JP リリースノートの非対称性は「他の人に使われて成長する」の対象が事実上日本語圏に絞られている可能性を示唆

## [2026-06-03] ingest | pKozaneba2025-08-26~27 の追加概念抽出 + 直前の framing 訂正

- nishio の指摘 1:[pKozaneba2025-08-26~27](../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md) には「1 万件デモ」以外にも有用な概念の切り口がある(7 つ提案 → 全部やる)
- nishio の指摘 2:Canvas プロトタイプは **Kozaneba 本流改修ではなくサイド実験** であることを忘れずに書く
- 直前の作業([Canvas 1 万件デモの拡張](themes/Canvas_1万件デモの拡張.md))で「本流改修と読める書き方」「Plan B 改造の範囲」と書いてしまった部分を訂正。framing を「サイド実験」「条件付きで適用可」に統一
- 新規 concept 3 本:
  - [大きな付箋](concepts/大きな付箋.md) — ズームアウトでも読める付箋。[なめらかな畳まれ](concepts/なめらかな畳まれ.md) と対の関係、人力 inverse-zoom title 相当
  - [密度の高さを大きさに変換して可視化](concepts/密度の高さを大きさに変換して可視化.md) — 2025-08 プロトタイプの中心可視化原則。Kernel Density Estimation との比較、パラメータトレードオフを記録
  - [内部構造がわかりやすい](concepts/内部構造がわかりやすい.md) — 可視化評価の肌感メトリック。半透明散布図が「内部構造で勝ったが見栄えで負けた」失敗事例
- 新規 theme 2 本:
  - [認知メタファのデザイン](themes/認知メタファのデザイン.md) — 既知メタファを借りる、抽象度を上げない、という Kozaneba 設計哲学。散布図/KDE/付箋の比較から内部構造・見栄え・既知メタファの 3 軸トレードオフを抽出
  - [人間が動かすから隙間ができる](themes/人間が動かすから隙間ができる.md) — 本流(人間駆動)とプロトタイプ(アルゴ駆動)の本質的な違いを言語化。[物理演算](concepts/物理演算.md) 禁忌と同系統、frame 抽象を機械的に統一しない理由
- 既存ページ更新:
  - [themes/Canvas 1 万件デモの拡張](themes/Canvas_1万件デモの拡張.md) — 全面改稿。冒頭に「サイド実験であって本流改修ではない」明示、拡張案は「続けるなら/知見を持ち帰るなら」の条件付き、「Plan B 改造の範囲」表記を削除
  - [themes/Canvas移行の検討](themes/Canvas移行の検討.md) — 2026-06-03 節をサイド実験 framing に修正、frame 抽象の機械的統一の危険性を追加
  - [themes/畳むUIの再設計](themes/畳むUIの再設計.md) — 広聴 AI セクションをサイド実験 framing に修正
  - [concepts/なめらかな畳まれ](concepts/なめらかな畳まれ.md) — [大きな付箋](concepts/大きな付箋.md) へのリンク追加
  - [concepts/広聴AI](concepts/広聴AI.md) — サイド実験としての Canvas プロトタイプ節を追加、新規 4 ページにリンク
  - [themes/Plan B 試行 2026-06](themes/Plan_B試行_2026-06.md) — 仮説の前史として 2025-08 サイド実験での AI 一発実装の体感を追加(ただし本流の局所改修とゼロから書く状況の差は本実験で測る、と但し書き)
- メモリ追加: `feedback_canvas_kozaneba_prototype.md`(feedback type)— 今後同じ framing ミスを繰り返さないため
- 主要な発見:
  - 「内部構造がわかりやすい / 見栄え / 既知メタファ」の 3 軸トレードオフは Kozaneba 設計判断を読む新しい軸として有効
  - 本流(人間駆動)とプロトタイプ(アルゴ駆動)の「隙間の意味」の違いは、[物理演算](concepts/物理演算.md) の「機械が動かしてはいけない」と同じ系統で、Kozaneba 設計思想の通奏低音として読み直せる
  - 「賢い AI Agent は過去実装を簡単に直す」仮説は 2025-08 のサイド実験での体感が起点で、Plan B 試行はゼロから書くのと既存に手を入れるのの差を測っている

## [2026-06-03] query | 1 万件 Canvas デモと 2026-06 サーベイの接続

- nishio の問い:scrapbox.io/nishio で記憶している「Kozaneba か チームみらい の可視化で 1 万件付箋を Canvas でズーム」デモが、最近の段階的詳細化 / グループ表示高サーベイで改良できそう、という勘の確認。
- 該当デモを特定:[pKozaneba2025-08-26~27](../raw/scrapbox_kozaneba/2025-08-26__pKozaneba2025-08-26~27.md) の Canvas プロトタイプ(`https://canvas-kozaneba-prototype.vercel.app/`)。動機は [広聴AI](concepts/広聴AI.md) のリーフノード = 付箋化、実装は Kozaneba 側の Canvas プロト。UMAP → NOTE_SIZE=120 グリッドスナップ → 螺旋分散 → 「密度の高さを大きさに変換」まで到達、ただし graphical zoom 止まり。
- サーベイ([畳むUIの再設計](themes/畳むUIの再設計.md))の **inverse-zoom title / AI 自動表札 / first-class frame / Hierarchical Edge Bundling** が直接効くと判定。「密度 → 大きさ」と「束 → 表札」を同じ semantic zoom 軸で統一でき、[pKozaneba2025-08-29](../raw/scrapbox_kozaneba/2025-08-29__pKozaneba2025-08-29.md) の「10000 件路線 vs 1000 件路線」を同じ frame + 表札抽象で両立させる道が見える。
- 新規 [themes/Canvas 1 万件デモの拡張](themes/Canvas_1万件デモの拡張.md) を作成し、実装順(inverse-zoom 試作 → AI 表札 → frame 一級化 → edge bundling)と残された問い(Plan A との関係、AI 表札の信頼性、Kernel Density Estimation との比較で立てた「1 つ 1 つが意見」が表札化で崩れる懸念)を整理。
- 既存ページ更新:
  - [themes/Canvas移行の検討](themes/Canvas移行の検討.md) — 末尾に「2026-06-03: 1 万件デモを 2026-06 サーベイで再評価」節を追加
  - [themes/畳むUIの再設計](themes/畳むUIの再設計.md) — 「広聴 AI 1 万件ケースへの適用」節と関連リンクを追加
  - [index.md](index.md) — 新ページをカタログに追加
