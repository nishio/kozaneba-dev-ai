---
title: Log
type: meta
created: 2026-05-16
updated: 2026-05-25
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
