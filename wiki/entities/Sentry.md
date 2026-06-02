---
title: Sentry
type: entity
created: 2026-06-03
updated: 2026-06-03
sources:
  - raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md
  - raw/scrapbox_kozaneba/2021-08-10__Kozaneba開発日記2021-08-10.md
  - raw/scrapbox_kozaneba/2021-08-16__Kozaneba開発日記2021-08-16.md
  - raw/scrapbox_kozaneba/2021-09-08__Kozaneba開発日記2021-09-09.md
  - work/kozaneba/package.json
  - work/kozaneba/src/index.tsx
  - work/kozaneba/src/initSentry.tsx
---

# Sentry

Sentry は Kozaneba の production 実行時に初期化されるエラー観測ツール。2021-08 の公開準備時点で「不特定多数の人が自分の見ていないところで使う」ための安全網として導入された。

現行コードでは [initSentry.tsx](../../work/kozaneba/src/initSentry.tsx) が `@sentry/react` と `@sentry/tracing` を使って初期化し、[index.tsx](../../work/kozaneba/src/index.tsx) の `initProduction()` から production のみで呼ばれる。

## 現行実装の事実

- [package.json](../../work/kozaneba/package.json) には `@sentry/react` と `@sentry/tracing` が残っている。
- [initSentry.tsx](../../work/kozaneba/src/initSentry.tsx) では DSN がハードコードされており、Sentry project は `o376998.ingest.sentry.io/5898295` に送信される。
- `beforeSend` で例外なら Sentry report dialog を表示する。
- `Manual Feedback` を含む message でも feedback 用 report dialog を出す。
- `beforeSend` 内で `getGlobal()` を `global` context として Sentry scope に設定している。現在の event に確実に付くかは実データで確認が必要だが、Kozaneba 状態を観測しようとした設計意図は残っている。
- `tracesSampleRate: 1.0` なので、performance event も本番で 100% sampling している可能性がある。

## 設計上の注意

2021-08-16 と 2021-09-09 の開発日記では、ユーザが試行錯誤中に起こす入力エラーや UserScript エラーを Sentry に送るべきではなく、ユーザ本人へフィードバックするべきだと整理されている。

したがって Sentry の issue は、そのまま修正 backlog にしてはいけない。まず「本体バグ」「入力 validation 不足」「UserScript / 拡張由来」「外部サービス / ネットワーク」「開発・テストノイズ」に分類する必要がある。

## Wiki 上の位置づけ

Sentry は [運用エラー観察](../themes/運用エラー観察.md) の一次ソース候補である。GitHub Issue や Cypress failure と違い、実ユーザまたは production 実行で実際に起きた障害を拾える。

## Sources

- [Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)
- [Kozaneba開発日記2021-08-10](../../raw/scrapbox_kozaneba/2021-08-10__Kozaneba開発日記2021-08-10.md)
- [Kozaneba開発日記2021-08-16](../../raw/scrapbox_kozaneba/2021-08-16__Kozaneba開発日記2021-08-16.md)
- [Kozaneba開発日記2021-09-09](../../raw/scrapbox_kozaneba/2021-09-08__Kozaneba開発日記2021-09-09.md)
- [package.json](../../work/kozaneba/package.json)
- [index.tsx](../../work/kozaneba/src/index.tsx)
- [initSentry.tsx](../../work/kozaneba/src/initSentry.tsx)
