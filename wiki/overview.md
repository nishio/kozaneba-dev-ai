---
title: Kozaneba 全体像
type: overview
created: 2026-05-16
updated: 2026-05-16
sources:
  - raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md
  - raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md
  - raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md
  - raw/init.txt
---

## Kozaneba とは

**Kozaneba**(こざねば)は、nishio がオープンソースで開発・無償提供している「**かんがえをまとめるためのデジタル文房具**」。Web アプリ([tutorial](https://kozaneba.netlify.app/))。

> 「KJ法的な手法はとても有益だけど紙でやるのには色々不便がある。よいデジタル文房具が欲しい」というモチベーションで作られた。
> — [かんがえをまとめるデジタル文房具Kozaneba](../raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md)

自分が考えをまとめるために使い、自分が必要だと思った機能を優先して実装している。主要機能:

- **グループを畳む**
- **重要なものを大きくする**
- **ものの間に関係の線を引く**

## 系譜

Kozaneba は単発のプロダクトではなく、長年の試行錯誤の系譜の上にある:

- **Regroup** — 情報断片を動かしながら頭を整理するプロセスを電子的に支援するために作った最初期版。試行錯誤で複雑化し、テスト容易性も低かった。
- **Movidea** — Regroup をゼロから作り直したもの。
- **Kozaneba** — Movidea の後継。テストしながら作り直され、新規ユーザ向けにチュートリアルも整備された。

詳しくは [Kozaneba の系譜](themes/系譜.md)(未作成)。

## なぜ作るのか(2021-08 の整理)

[Kozanebaを作ることで何がどうなればいいのか](../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md) で nishio 自身が Keichobot との対話を通じて整理した結論:

> 当初「自分が使いたいツールを作るのだ」と考えていたが、それなら「新規ユーザにわかりにくいから作り直そう」とは整合しない。掘り下げると「**多くの人に使われること**」に価値を感じていた。さらに掘ると「自分が紙でやってたことを Regroup でやれるようになり有益だと思っているが、それが多くの人に使われることで『ひとりよがりの思い込み』ではないことを確認したい」「自分以外の人が使うと、予想しなかった使い方をして、その刺激で僕の思考が発展する」。
>
> 結論: **他の人に使われることによって、より良くなるといい。** ツールは「子供」のメタファー — 旅に出したほうが成長する。そのためには公開され、背景知識の異なる多様な人に使われ、使用に伴う情報が作者にフィードバックされる必要がある。

このフレームは以降の設計判断の通奏低音になっている。

## このプロジェクト(kozaneba-dev-ai)の位置づけ

数年にわたる断続的開発で nishio 自身の記憶が曖昧化してきたため、設計過程の思考メモを LLM 主導で構造化された wiki に整理する。最終目標は「現状の Kozaneba を改善するか、新しいものを作るか」を判断できる土台を作ること。

詳細は [CLAUDE.md](../CLAUDE.md) を参照。

## Sources

- [raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md](../raw/scrapbox_kozaneba/2021-08-20__かんがえをまとめるデジタル文房具Kozaneba.md)
- [raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md](../raw/scrapbox_kozaneba/2021-09-02__Kozaneba.md)
- [raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md](../raw/scrapbox_kozaneba/2021-08-08__Kozanebaを作ることで何がどうなればいいのか.md)
- [raw/init.txt](../raw/init.txt)
