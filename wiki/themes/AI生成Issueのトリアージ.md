---
title: AI生成Issueのトリアージ
type: theme
created: 2026-06-03
updated: 2026-06-03
sources:
  - https://github.com/nishio/kozaneba/issues
  - work/kozaneba/package.json
  - work/kozaneba/README.md
  - work/kozaneba/browser_extension/kintone_tampermonkey.js
  - work/kozaneba/src/App/KeyboardShortcut.tsx
  - work/kozaneba/src/utils/dev.ts
  - work/kozaneba/scripts/codex-preflight.sh
  - wiki/themes/Plan_B試行_2026-06.md
  - wiki/themes/テスト改善計画.md
---

# AI生成Issueのトリアージ

## 何が起きたか

2026-06-03 に GitHub Issues の全文を読み、`nishio/kozaneba-dev-ai` ではなく本体リポジトリ `nishio/kozaneba` の open Issue 20 件を整理した。20 件はいずれも 2025-04-26 に `app/devin-ai-integration` が作成したもので、コメントはなかった。

全文を読むと、これらは具体的な障害報告というより、AI が一般的な改善カテゴリを分割して作った backlog だった。内容には有用な観点も含まれるが、現在のコード事実・優先順位・実行単位に照らすと、そのまま「やること」にしてはいけない。

整理後に残した open Issue は次の 5 件:

- [#4 本番環境のコンソールログ削除](https://github.com/nishio/kozaneba/issues/4)
- [#12 ブラウザ拡張機能のドキュメント整備](https://github.com/nishio/kozaneba/issues/12)
- [#13 CI/CD パイプラインの構築](https://github.com/nishio/kozaneba/issues/13)
- [#17 テストカバレッジの向上](https://github.com/nishio/kozaneba/issues/17)
- [#25 コンポーネントレンダリングのプロファイリング実施](https://github.com/nishio/kozaneba/issues/25)

残り 15 件は、古い前提、性能系の重複、または広すぎる一般論として理由コメント付きで close した。

## 得られた知見

AI が作った Issue は、**提案の束** として読むべきであり、**実行可能な作業リスト** として読んではいけない。

Issue 化されていると、見た目は backlog のように見える。しかし今回の 20 件は、次のように性質が混ざっていた。

- 現在のコード事実と合っていないもの
- 有効な改善だが、範囲が大きすぎるもの
- 性能問題の「手段」を先に決めているもの
- 製品方針や対象ユーザーが決まらないと判断できないもの
- 小さく実行でき、今の改善計画に接続できるもの

つまり、Issue の全文を読む作業は単なる事務処理ではなく、[Plan B 試行 2026-06](Plan_B試行_2026-06.md) における「現 Kozaneba 改造をどの粒度で進めるか」を決める設計判断である。

## トリアージ基準

### 1. まずコード事実と照合する

Issue #6 は「React 17 / Firebase 8 が古い」という前提だったが、現在の [package.json](../../work/kozaneba/package.json) では React 18.2 / Firebase 12.14 になっていた。Issue #8 は `App/KeyboardShortcut.tsx` に cleanup がないことを具体例にしていたが、現在の [KeyboardShortcut.tsx](../../work/kozaneba/src/App/KeyboardShortcut.tsx) は cleanup 関数を返している。

この種の Issue は、たとえ一般論として正しくても、現在の作業対象としては閉じる。必要になったら、現在の `npm outdated`、実際の脆弱性、または具体的な `useEffect` の漏れに基づいて切り直す。

### 2. 小さく実行できるものだけ残す

残した Issue は、現在のコードに直接根拠があり、初手を小さく切れる。

- #4: `console.log` は実際に多数残っており、[dev_log](../../work/kozaneba/src/utils/dev.ts) も既にある
- #12: [browser_extension/kintone_tampermonkey.js](../../work/kozaneba/browser_extension/kintone_tampermonkey.js) は存在するが [README](../../work/kozaneba/README.md) に説明がない
- #13: `.github/workflows` がなく、[codex-preflight](../../work/kozaneba/scripts/codex-preflight.sh) などを CI 化する入口がある
- #17: Cypress E2E はあるが unit test は薄く、[テスト改善計画](テスト改善計画.md) に接続できる
- #25: 性能改善の前に、まずプロファイルという観測作業が必要

この 5 件は「完璧な改善テーマ」ではなく、**次の一手が明確な Issue** である。

### 3. 性能改善は手段ではなく観測から始める

性能系 Issue は #10 / #23 / #24 / #25 / #26 / #27 に分かれていた。

そのうち `React.memo`、仮想化、画像 lazy loading、Web Worker はすべて手段である。どの手段が必要かは、`ItemCanvas`、`ids_to_dom`、`useGlobal()` の購読、Gyazo / Scrapbox 画像表示、物理計算などを測ってからでないと決められない。

特に Kozaneba は通常の縦リストではなく、キャンバス上に小札・グループ・線・選択状態を配置する UI である。`react-window` のようなリスト仮想化をそのまま導入する発想は、問題設定を取り違える可能性がある。

したがって、性能系は #25 に集約し、プロファイル結果に基づいて必要な小 Issue を再発行する形にした。

### 4. UX / i18n / accessibility は対象フローなしに進めない

accessibility、mobile、i18n、onboarding はどれも重要なテーマである。しかし今回の Issue は、具体的な対象ユーザー、画面、操作、失敗条件がない一般論だった。

これらは「やらない」のではなく、次のような形に落ちたときに扱う。

- キーボードだけで小札作成まで進める
- スマホ幅で小札作成ダイアログが使える
- 既存チュートリアルで離脱する箇所を直す
- 対象言語と対象画面を決めて翻訳する

Kozaneba のような思考支援ツールでは、一般的な UX 施策を足すより、**どの思考行為を妨げているか** を先に特定する方が重要である。

### 5. 大規模リファクタは単独Issueにしない

`any` 削減、命名規則統一、グローバル状態管理の改善は、すべて価値がある。しかし、それ自体を独立した大きな作業にすると、差分ノイズが増え、どのユーザー価値に効いたか説明しにくい。

これらは、機能改修・テスト追加・性能測定の中で、対象ファイルに触ったときに局所的に改善する方がよい。状態管理の再設計が必要になる場合も、まず性能プロファイルや具体的な保守上の痛みを出してから切る。

## Plan B への含意

今回の学びは、[AIエンジニアたち](../entities/AIエンジニアたち.md) を使った Kozaneba 改善で、AI Agent に何を任せるかの境界を示している。

AI Agent は Issue を大量に生成できる。しかし、生成された Issue は「今やるべきこと」を自動的には表さない。むしろ、人間または別の LLM が次を行う必要がある。

1. Issue 全文を読む
2. 現在のコード事実と照合する
3. 測定が必要なものと実装できるものを分ける
4. 広すぎるものを閉じる、または観測 Issue に集約する
5. 残す Issue には次の一手をコメントする

この作業は backlog の掃除ではなく、**AI が出した可能性の束を、Kozaneba の現在地に合わせて再構造化する行為** である。

[テスト改善計画](テスト改善計画.md) と同じく、ここでも重要なのは「全部を実行する」ことではない。何を required gate にするか、何を non-required 観測に回すか、何を閉じて wiki に判断理由だけ残すかを分けることである。

## 次の運用ルール

今後、AI が作った Issue や改善提案を扱うときは、次の順序にする。

1. Issue 本文とコメントを全文読む
2. `work/kozaneba/` を最新化し、現在のコード事実と照合する
3. 「実装」「計測」「方針決定」「閉じる」に分類する
4. 実装に残す Issue は、最初の PR スコープをコメントしてから着手する
5. 広い Issue を閉じるときは、消すのではなく、再発行条件をコメントする
6. 判断から得た知見は wiki に戻す

GitHub Issues は実行キュー、wiki は判断理由の保存場所である。Issue を閉じても、判断理由を wiki に残せば、将来の自分は「なぜやらなかったか」「いつならやるべきか」を再利用できる。

## Sources

- [nishio/kozaneba Issues](https://github.com/nishio/kozaneba/issues)
- [package.json](../../work/kozaneba/package.json)
- [README.md](../../work/kozaneba/README.md)
- [kintone_tampermonkey.js](../../work/kozaneba/browser_extension/kintone_tampermonkey.js)
- [KeyboardShortcut.tsx](../../work/kozaneba/src/App/KeyboardShortcut.tsx)
- [dev.ts](../../work/kozaneba/src/utils/dev.ts)
- [codex-preflight.sh](../../work/kozaneba/scripts/codex-preflight.sh)
- [Plan B 試行 2026-06](Plan_B試行_2026-06.md)
- [テスト改善計画](テスト改善計画.md)
