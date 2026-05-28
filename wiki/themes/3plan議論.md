---
title: 3 Plan 議論(Kozaneba の次)
type: theme
created: 2026-05-16
updated: 2026-05-19
sources:
  - チャット議論 2026-05-16(nishio が 3 Plan を提示し、Claude との対話で「道具改造 + 新規プロダクト」の二段構えに整理)
---

# 3 Plan 議論 — Kozaneba の次に何を作るか

## 出発点(nishio が提示した 3 Plan)

2026-05-16、nishio が「今考えていること」として 3 つの方向を提示:

**Plan A: Keichobot の発展形として個人向け言語化支援システム**
- 人間と AI の対話が主たる操作方法
- 選択肢 / 違和感ボタン / 付箋ドラッグは「まだ言語化できていないものをデータ化する手段」という位置付け

**Plan B: 300 件の文章からの知識獲得支援**
- 想定ユーザが多そう、直近の出口がある
- 「1 万件可視化」より現在のデータスケールに近い、現実装の修正でカバーできる

**Plan C: Kozaneba読書にフォーカス**
- Plan A の「自分の中のまだ言語化されていないものを取り出す」は、自分の中に価値があると信じられない人には刺さらない
- 書籍の中に価値があって自分がうまく取り入れられていないと感じる人は多い

## Claude 側の最初の整理: 3 Plan は排他か

3 Plan を [関係場](../concepts/関係場.md) の Pipeline に乗せると、Situation 由来の違いに見える:

| Plan | Situation 由来 | Aspect 化 | Projection |
|---|---|---|---|
| A 言語化支援 | 対話履歴 | 違和感ボタン/付箋 = 前言語的データ化 | こざね配置 |
| B 300 件知識獲得 | 元の 300 文書 | 文書→こざね分解 | キャンバス |
| C Kozaneba読書 | 書籍 | こざね生成 | キャンバス |

→ どの Plan も [源の長文](../concepts/源の長文.md)(Situation データモデル)の拡張を共通基盤として必要とする。

Plan C は Plan B の特殊ケースとして包含できる:
- C: Situation 数 1〜数件、各 Situation が極端に長い
- B: Situation 数 数百、各 Situation は中程度

技術選択としては B に内包される(差は入力方法とオンボーディングのみ)。

## nishio の応答で大きく変わった構図

質問 1「直近の出口は?」→ **「nishio 自身の 300 件作業を加速」**(= Plan B)

質問 2「Plan A のときの Keichobot/いどばた との関係は?」→ **「Keichobot/いどばた と Kozaneba を参考にしつつ、全く新しいサービスが作られる」**

この 2 つで構図が**「3 Plan の選択」から「道具改造(B)+ 新規プロダクト(A)の二段構え」**に転換した。

「Kozaneba を改善するか作り直すか」の長年の問い([themes/系譜](系譜.md) / [themes/Canvas移行の検討](Canvas移行の検討.md))に対する答えとして、**両方を並行する**:

| | 内容 | 時間軸 | ユーザ |
|---|---|---|---|
| **Plan B(現 Kozaneba 改造)** | 300 件作業の加速、[源の長文](../concepts/源の長文.md) データモデル拡張 | 直近 | nishio 自身(ドッグフーディング) |
| **Plan A(新規サービス)** | Keichobot + いどばた + Kozaneba を参考にした次世代 | Plan B の知見を貯めた後 | 専門家向け |
| Plan C | Plan A のマーケティング・オプション(読書家層) | 後で考えればよい | 読書家層 |

Plan A が「Kozaneba の発展」ではなく「全く新しいサービス」になった瞬間、Plan B はもう「次の Kozaneba」ではなく「**Plan A への踏み台**」として位置づけ直せる。

## Plan A の輪郭(llm-wiki 側の概念を組み合わせると)

Plan A はおそらく [ConnectingDots](../../../llm-wiki/wiki/entities/connecting-dots.md) の最初の本実装になる。llm-wiki 側で言語化されている概念を組み合わせると素描が出る:

- **データモデル**: Dots / Relations / Stories / Views 4 層 + [identity-without-name](../../../llm-wiki/wiki/concepts/identity-without-name.md)(知番のように名前と対象を分離)
- **対話レイヤ**(Keichobot/いどばた 継承): [pre-linguistic-structuring](../../../llm-wiki/wiki/concepts/pre-linguistic-structuring.md) の前言語層を [Clean Relation Elicitation](Clean_Relation_Elicitation.md) の Situation → Aspect → Relation Field → Projection で引き出す
- **AI 統合原則**: [structure-as-hypothesis](../../../llm-wiki/wiki/concepts/structure-as-hypothesis.md)(候補は AI、確定は人間)+ Inbox 化パイプライン
- **空間レイヤ**(Kozaneba 継承): こざね配置 + 関係場の Projection View
- **関係の保持**: [relation-flattening](../../../llm-wiki/wiki/concepts/relation-flattening.md) を意識的に避ける設計(平坦化せず、根拠と向きと文脈を保つ)

= **「対話で Situation を引き出し、AI が候補化、人間が空間で確定、Stories として読み筋を立てる」**システム。Keichobot(対話)+ Kozaneba(空間)+ MindTrellis(AI候補)+ ConnectingDots(層分離)の合成。

「LLM 時代の知識基盤」([knowledge-substrate](../../../llm-wiki/wiki/concepts/knowledge-substrate.md))という、ページ群ではなく断片・主張・問い・仮説・根拠・関係・履歴のネットワークを基盤にする構想とも接続しうる。

## Plan B 期間で「Plan A 要求発見」のために記録すべきもの

Plan B を単に「作業を速くする」だけで使うと Plan A への翻訳が消える。意識的に記録する 5 項目:

1. **現 Kozaneba で「やりたかったができなかった」操作** — その都度 raw/ にメモ
2. **AI と対話したかった場面** — 「ここで対話相手がいれば言語化できた」の瞬間
3. **関係を引きたかったが言語化が早すぎて諦めた瞬間** — relation-flattening を踏まないための実例
4. **同じこざねを違う Story で再利用したかった場面** — ConnectingDots の Stories 層の必要性
5. **Situation(源の長文)から複数こざねが生まれた経路** — 1 長文 → N こざね のデータモデル検証

この 5 項目を Plan B 作業中に記録すれば、Plan A の要求仕様が自分で書きながら勝手に貯まる。

## 注意したい論点

### 「自分が一番のユーザ」の罠

Plan B のドッグフーディングは強力だが、Plan A の専門家向け設計に翻訳するときに「nishio にしか刺さらない設計」を持ち込みやすい。Plan B 期間でも「自分でない誰か」の像を 1 人持っておく(大学院生 / 研究者 / 編集者 / コンサル 等)。

### 並列 vs deprecate(未決)

Plan A が新規でも、Keichobot / いどばた / Kozaneba を:
- **並列で残す** → 4 つ並ぶ。ブランド・メンテナンス・ユーザ移行のコスト
- **deprecate する** → 何を捨て、いつ捨てるか
- **段階的に吸収** → Plan A が成熟したら旧 3 つを順次クローズ

この判断は Plan A の輪郭が固まってから決めればよいが、忘れずに保留中の論点として置く。

### Plan C は「いつでもオプションとして拾える」

Plan C は B/A のどちらかを進めた後で、マーケティング・オンボーディング層として後付け可能。

「自分の中に価値があると信じられない人」が「外にあると思っていたものが自分の解釈だった」と気づく遷移パスを設計すれば、C → A への自然な動線になる。これは Plan A の入口戦略として後で生きる。

### 「全く新しいサービス」を立ち上げる重み

Plan A は単なる機能追加ではなく新規サービス。コードベース、データモデル、ブランド、ドメイン名、課金、ユーザサポート、ドキュメント等すべて新規。Plan B 期間中に Plan A の要求が「現 Kozaneba の改造で済むかどうか」も再評価される可能性がある(= Plan A が不要になる、または逆に Plan B が中途半端と判明する)。

## 関連ページ

- [系譜](系譜.md) — grouping → Regroup → Movidea → Kozaneba → Canvas検討 の延長線上に Plan A が来る
- [なぜ作るのか](なぜ作るのか.md) — Plan A の動機は「人類の知的能力強化」(2024 以降の動機)に近い
- [Canvas移行の検討](Canvas移行の検討.md) — 「分けるか統合するか」の議論。本ページで「両方やる」に着地
- [活用されなかった機能](活用されなかった機能.md) — Plan A 設計時に「移植しない機能」の参考
- [断片中心から関係中心へ](断片中心から関係中心へ.md) — Plan A は関係中心の上に立つ
- [関係を第一級にする](関係を第一級にする.md) — Plan A のデータモデル直接の根拠
- [Clean Relation Elicitation](Clean_Relation_Elicitation.md) — Plan A の対話レイヤ設計の出発点
- [Kozaneba vs Keichobot](Kozaneba_vs_Keichobot.md) — Plan A は分業を解体し連続体化する
- [こざね](../concepts/こざね.md) / [源の長文](../concepts/源の長文.md) — Plan B/A 共通のデータモデル拡張
- [関係場](../concepts/関係場.md) — Plan A のパイプラインのベース
- [sources/llm-wiki-cross-reference](../sources/llm-wiki-cross-reference.md) — Plan A 設計に効く llm-wiki 側概念の索引

## Open Questions

- Plan A の「新規サービス」は Kozaneba を deprecate するのか並列にするのか — Plan A 輪郭固まり後に判断
- Plan B 期間の長さの目安(数週間 / 数ヶ月 / 半年)
- Plan A の最小実装(MVP)の輪郭 — ConnectingDots の 4 週間アクションプラン([gpt-mindtrellis-connectingdots-20260503](../../../llm-wiki/wiki/sources/gpt-mindtrellis-connectingdots-20260503.md))が出発点になりうるか
- Plan B で記録された 5 項目を Plan A 要求仕様に変換するタイミング・方法
- Plan A の「専門家ユーザ」の具体像(誰を想定するか)
- Plan C を Plan A の入口戦略として組み込むのか、独立サービスにするのか

## 2026-05-19: このページの先にある整理

本ページは「Plan B + Plan A の二段構え」に着地したページ。その後、さらに一段抽象化した整理として [3つのストーリー比較](3つのストーリー比較.md) を作成した。

- 本ページの役割: **何を並行するか** を決める
- [3つのストーリー比較](3つのストーリー比較.md) の役割: **3 方向の違いは何か、各 MVP は何か、どの観察が出たらどこへ進むか** を明文化する

特に重要なのは、

- 改善ストーリー = Plan B を進めるための直接の作業路線
- 似たものを新しく作るストーリー = Plan A と Plan B の中間にある「Kozaneba の本質を保った次世代」仮説
- 全く新しいものを作るストーリー = 本ページで言う Plan A を、入口レベルから別物として実装する案

という 3 分割。これにより、Plan A と Plan B のあいだに「似たものを新規実装する」中間案があることが明示化された。
