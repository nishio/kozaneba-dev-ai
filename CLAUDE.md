## このリポジトリの目的

nishio が長年にわたり開発してきた「かんがえをまとめるデジタル文房具 Kozaneba」の設計過程の思考メモを、LLM 主導で構造化された wiki に育てていく。最終目標は「Kozaneba を改善するか / 新しいものを作り直すか」を未来の自分が判断できる土台を作ること。

設計思想は [llm-wiki.md](llm-wiki.md) に従う。生ソースは不変、wiki は LLM が育てる構造化レイヤ、`CLAUDE.md`(このファイル)がスキーマ。

## ディレクトリ構成

```
.
├── CLAUDE.md            このファイル(スキーマ)
├── llm-wiki.md          採用しているパターンの説明(参考、編集しない)
├── raw/                 生ソース(不変)
│   ├── init.txt         プロジェクトの初期指示
│   ├── nishio.json      Scrapbox /nishio エクスポート(57MB、25633ページ)
│   └── scrapbox_kozaneba/  nishio.json から "Kozaneba/こざねば" を含む 403 ページを抽出したもの
│       ├── _manifest.json     抽出ページの一覧(title / created / updated / in_title / file)
│       └── YYYY-MM-DD__<title>.md  各ページ
├── work/                実装確認用の local clone を置く場所(gitignored)
│   └── kozaneba/        `git clone git@github.com:nishio/kozaneba.git` した本体コード
└── wiki/                LLM 管理レイヤ
    ├── index.md         全ページのカタログ(コンテンツ指向)
    ├── log.md           時系列の作業ログ(append-only)
    ├── overview.md      Kozaneba の全体像
    ├── entities/        固有名詞(プロダクト、人、ツール)のページ
    ├── concepts/        概念のページ(KJ法、こざね、ねりねり、IOFI、…)
    ├── themes/          テーマ別の統合・考察ページ(なぜ作るのか、UX、…)
    └── sources/         必要に応じて、個別ソースの要約ページ
```

## 各レイヤの責務

**raw/**
- nishio が用意した生ソース。LLM は読むだけで、絶対に変更しない。
- 新しい生ソース(Scrapbox 再エクスポート、設計メモ、コード断片、外部記事のクリップ等)はここに追加される。

**work/**
- Kozaneba 本体コードの local clone を置く。LLM がコード構造や現行実装を確認するときの一次参照。
- 標準の置き場所は `work/kozaneba/`。必要なら `git fetch origin && git pull --ff-only` で最新化してから参照する。
- `work/` はローカル作業領域なので git には含めない。
- 実装・再現確認・テストなど新しい作業を始める前に、対象 clone の `git status --short` を確認する。dirty な場合は既存変更をユーザー作業として扱い、そこで作業を始めず、`git worktree add --detach work/<目的名> HEAD` などで clean worktree を作って隔離して進める。

**wiki/**
- LLM がすべて生成・更新する。手書きしない。
- markdown 内のリンクは Obsidian 互換の相対パス `[表示名](../entities/Kozaneba.md)` 形式を基本とする。
- ページ冒頭には YAML frontmatter を付ける:
  ```
  ---
  title: <ページタイトル>
  type: entity | concept | theme | source | overview | meta
  created: YYYY-MM-DD
  updated: YYYY-MM-DD
  sources:
    - raw/scrapbox_kozaneba/YYYY-MM-DD__xxx.md
  ---
  ```
- ページ末尾には「Sources」セクションを置き、参照した生ソースへの相対リンクを列挙する。

## ワークフロー

### Ingest(取り込み)
新しい生ソースが追加されたら以下を行う:
1. コード由来の更新や確認が必要なら、先に `work/kozaneba/` を `git fetch origin && git pull --ff-only` で最新化する。
2. ソースを読み、要点を user に共有(必要なら短くチャットで議論)。
3. 該当する entity / concept / theme ページを更新。新規概念は新ページを作成。
4. `wiki/index.md` を更新。
5. `wiki/log.md` に append: `## [YYYY-MM-DD] ingest | <ソース名>` の見出しと、何を更新したかの 3〜5 行サマリ。
6. 1 ソースで 5〜15 ページに触れることがある。全ての関連ページを更新する。

### Query(問い合わせ)
user が wiki に対して問いを投げたら:
1. `wiki/index.md` をまず読んで関連ページを特定。
2. 質問が実装や現行コード構造に関わる場合は、`work/kozaneba/` を一次参照として確認する。
3. 必要なページを読んで合成。回答には参照ページへのリンクを必ず含める。
4. 価値ある合成・比較・気づきは新規 wiki ページとして保存することを提案する(チャット履歴に消さない)。
5. `wiki/log.md` に append: `## [YYYY-MM-DD] query | <問い>` と要約・生成したページへのリンク。

### Lint(健全性チェック)
定期的に user に依頼されたら:
- ページ間の矛盾、古くなった主張、孤立ページ、未作成だが頻出する概念、欠けている相互参照を洗い出す。
- 結果は提案として返し、user の承認のもとで反映する。
- `wiki/log.md` に append: `## [YYYY-MM-DD] lint` と所見・対応。

## 命名規約

- entity / concept ページ:`wiki/entities/<Name>.md`、`wiki/concepts/<名前>.md`
  - 名前は元ソースの表記を尊重する(英名は英名、日本語は日本語)。
  - ファイル名にスペースは含めない。区切りはハイフンまたはアンダースコア。
- 日付は ISO 形式 `YYYY-MM-DD`。
- log の見出しは `## [YYYY-MM-DD] <action> | <subject>` で統一(`grep "^## \[" wiki/log.md` でパース可能にするため)。
- **raw/ 直下のファイル名は内容を表す名前にする**(プレースホルダ名 `a.txt` のままにしない)。
  - 推奨形式:`raw/<内容を表す名前>.md`、または日付情報があるなら `raw/<YYYY-MM>_<内容>.md`。
  - 1 ソースの中に複数ラウンド・複数 AI とのやりとりが含まれる場合は、その代表テーマを名前にする(例: `raw/関係UI議論_GPT.md`)。
  - 命名は ingest 工程の最初に行う。**wiki ページから raw/ を参照した後にリネームすると 10〜50 箇所の参照を更新する手間が発生する**ので注意。
  - サブディレクトリ(`raw/scrapbox_kozaneba/` など)内のファイル命名規約はそのディレクトリの慣習に従う(例: scrapbox 系は `YYYY-MM-DD__<title>.md`)。

## 進め方の方針

- nishio は Kozaneba の作者本人なので、wiki の内容が事実と違うときは即座に指摘してくれる。LLM 側は推測を断言せず、根拠ソースを示す。
- raw に大量のページがあるので、最初から全部処理しない。中心概念(Kozaneba そのもの、前身の Regroup / Movidea、関連ツール Keichobot / Scrapbox、設計思想の KJ法 / こざね / ねりねり 等)から段階的に整備する。
- 実装断定が必要なときは `work/kozaneba/` の local clone を優先し、Scrapbox 上の記述は設計意図や履歴の補助線として扱う。
- 「未来により良いものを生み出す」のが究極目的なので、設計判断・後悔・繰り返し出てくる課題は themes/ に明示的に蓄積する。

## 参考

- [llm-wiki.md](llm-wiki.md) — このリポジトリが従っているパターン
- [raw/init.txt](raw/init.txt) — nishio による初期指示
