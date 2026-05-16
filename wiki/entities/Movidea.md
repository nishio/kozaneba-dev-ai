---
title: Movidea
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md
  - raw/scrapbox_kozaneba/2021-08-10__2021-08-08Kozaneba中間発表に向けたまとめ.md
  - raw/scrapbox_kozaneba/2023-01-11__Kozanebaの開発環境を作る.md
---

## 定義

**Movidea** は [Regroup](Regroup.md) をゼロから作り直したもので、後に [Kozaneba](Kozaneba.md) にリネームされた。

## 開発背景

Regroup の二つの問題(コードの複雑化、ユーザフローの複雑さ)を解消するため、テストを書きながら作り直された。

- Cypress と React-N の組み合わせでテスト可能にする(2021-07-02)
- `immer` で状態更新できるようにしたお陰で、前バージョンでは難しくて放置していた仕様が「あっさり実装できた」
- Firebase Auth / Firestore も Cypress でテスト
- 2021-08-06 に Movidea から **Kozaneba** へリネーム

開発環境を作り直したログ(2023-01-11)では、Kozaneba ローカル起動時のメッセージに `You can now view movidea in the browser.` が残っており、内部的には Movidea という名前がまだ残っている。

## 関連

- 前: [Regroup](Regroup.md)
- 後: [Kozaneba](Kozaneba.md)
- プロジェクトメモ: `[pMovidea]`(本リポジトリ未取り込み)

## Sources

- [Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)
- [2021-08-08Kozaneba中間発表に向けたまとめ](../../raw/scrapbox_kozaneba/2021-08-10__2021-08-08Kozaneba中間発表に向けたまとめ.md)
- [Kozanebaの開発環境を作る](../../raw/scrapbox_kozaneba/2023-01-11__Kozanebaの開発環境を作る.md)
