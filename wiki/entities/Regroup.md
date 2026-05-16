---
title: Regroup
type: entity
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md
  - raw/scrapbox_kozaneba/2021-08-10__2021-08-08Kozaneba中間発表に向けたまとめ.md
  - raw/scrapbox_kozaneba/2022-04-16__悟りに至るプロセスとしてのKozaneba.md
---

## 定義

**Regroup** は nishio が作った [Kozaneba](Kozaneba.md) の前々身。「情報断片を動かしながら頭を整理するプロセスを電子的に支援したい」と思って作られた。前身は `grouping` で、系譜は `grouping → Regroup → [Movidea](Movidea.md) → Kozaneba`。

## 抱えていた問題

試行錯誤しながら作っていたため、以下の問題が顕在化した:

- **コードの複雑化** — テストコードがなかったため、ある程度の規模に達した時点で機能追加が困難になった。テストしやすく作られてもいなかったので、その状態からテストを足すのも大変。
- **新規ユーザのフローの複雑さ** — 使い始めるまでのステップが複雑で、解説の文章を書こうとしても長くなる。「どう考えても使いにくい」と nishio 自身が判断。

この二つの問題のせいで「[Movidea](Movidea.md) としてゼロから作り直す」決断につながった。

## 関連

- データフォーマット: Regroup の JSON を Movidea/Kozaneba にインポートする経路が存在した(2021-07-02 時点で実装)
- 中間発表(2021-08-08)では「もう Regroup の話はしなくてよい」と nishio 自身が整理している

## Sources

- [Kozanebaを作ることで何がどうなればいいのか](../../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)
- [2021-08-08Kozaneba中間発表に向けたまとめ](../../raw/scrapbox_kozaneba/2021-08-10__2021-08-08Kozaneba中間発表に向けたまとめ.md)
- [悟りに至るプロセスとしてのKozaneba](../../raw/scrapbox_kozaneba/2022-04-16__悟りに至るプロセスとしてのKozaneba.md)
