# PKGBUILD-uim-skk-utf8

`uim-skk`をutf-8化するためのパッチ集、および PKGBUILD です。本来ならutf-8対応を「追加」する形のパッチにして [uim/uim](https://github.com/uim/uim) にプルリクエストをお送りすべきなのですが、本パッチは上書きして`uim-skk`でeuc-jpを使えなくしてしまうため、このような形になりました。

2025年02月17日追記：PKGBUILD内にあったコード変換のプロセスを削除し、変換済みのファイルをパッチで追加する形にしました。これで他のディストリビューションに移植しやすくなります。

### `*.scm`

`*.scm`の修正については次の方々の記事を利用させて頂きました。ありがとうございました。

* [「𠁣」の文字をどのようにキーボード入力するか](https://harakire.tripod.com/junkies/non-bmp-keyb.html)

* [パッチを当ててuim-skkをUTF-8化する](https://keens.github.io/blog/2019/10/20/patchiwoateteuim_skkwoutf_8kasuru/)

### `skk.c`

`skk.c`における「数値変換」のコードは漢数字などが2バイト文字であることが前提になっているため、これを3バイトに書き換えました。なお`uim-skk`の数値変換でサポートしている漢数字の単位は「京」までですが、続く「垓、𥝱、穣、溝、澗、…」のうちの「𥝱」は utf-8 では4バイト[^1]であるため、ここまで拡張するには更なる書き換えが必要です。

2022年09月28日追記：「垓」までは使えるようにしました。

### TODO

* 他のディストリビューションに移植するために必要な解説を書く。
* ~コード変換などの処理をPKGBUILDから外部スクリプトに移動して、他のディストリビューションに移植しやすくする。~
* 個人の設定ファイルや辞書ファイルの変更についての解説を書く。

[^1]: これについては拙作LaTeXパッケージ[`Xkansuji`](https://github.com/tattsan/xkansuji)の[解説](https://github.com/tattsan/xkansuji/blob/master/jousu.pdf)の1ページ目を参照。
