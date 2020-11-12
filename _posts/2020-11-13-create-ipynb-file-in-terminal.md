---
title: ipynbファイルをターミナルから作成しよう
categories: [Blogging, Tutorial]
tags: [ipynb, command, Terminal]
date: 2020-11-13 00:45:00 +0900
---
# TL;DR

STEP 1: 以下のファイルを作成し好きなところへ配置します．(ファイル名がコマンド名となります)

STEP 2: PATHを通して実行権限(`+x`)を付与します．

以上です．

## 使用方法
ファイル名を指定する
```terminal
$ mkipynb hoge piyo
$ ls
hoge.ipynb  piyo.ipynb
```

ファイル名を指定しない
```terminal
$ mkipynb
$ ls
Untitled.ipynb
```

## コマンドの実装

`~/command/mkipynb`
```shell
#!/bin/sh

if [ $# = 0 ]; then
    echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}' > Untitled.ipynb
else
    for i in "$@"
    do
        if [ "${#i}" -gt 255 ]; then
            echo 'mkipynb: cannot create' "'$i'" ': File name too long'
        else
            echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}' > "$i".ipynb
        fi
    done
fi

exit 0
```

# ipynbファイルとは
以下の画像のようにJupyter Notebookから開くとセル形式で表示されますが，ipynbファイルの実体はJSON形式で記述されたファイルです．

![Jupyter Notebookから開く](/assets/img/posts/2020-11-13-create-ipynb-file-in-terminal/jupyter-notebook.png)

実際の空の`Untitled.ipynb`ファイルの中身は以下のようになっています．

```json
{
 "cells": [],
 "metadata": {},
 "nbformat": 4,
 "nbformat_minor": 4
}
```

つまり，これを記述した`.ipynb`ファイルを用意すれば良いということです．

# echo コマンドでファイルに出力する
ipynbファイルの実体がわかったところで，どのようにしてファイルを作成するかを見ていきます．

シンプルな方法として，`echo`コマンドを用いてファイルに出力させれば良さそうです．

```terminal
$ echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}'
```

echoコマンドの結果をファイルに出力するには，上のコマンドの末尾に`> filename.ext`を追加します．

よって，`Untitled.ipynb`を作成するコマンドは次のようになります．

```terminal
$ echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}' > Untitled.ipynb
```

# ipynbファイルを生成する自作コマンドを作成しよう
毎回のように`Untitled.ipynb`で生成されてリネームするのは正直なところ面倒くさいです．
また，複数のファイルを同時に作成したい場合もあるでしょう．

そこで，これらの要望を実現するために，コマンドを自作しようということです．

コマンドを自作するのは簡単で，拡張子なしのファイルにシェルスクリプトを書くだけです．
この時，ファイル名がコマンド名となります．

今回は`~/command`以下に`mkipynb`として作成します．
ここでは`~/command`ディレクトリにはPATHが通っているものとします．
PATHの通し方は[こちら](https://qiita.com/Naggi-Goishi/items/2c49ea50602ea80bf015)が参考になると思います．

ファイルの一行目には以下のように記述します．

`~/command/mkipynb`
```shell
#!/bin/sh
```

あとは，要望をシェルスクリプトで記述するだけです．

## 作成したコマンドの構造

今回は，引数としてファイル名を指定する方式を採用し，引数が省略された場合は`Untitled.ipynb`とするようにしました．

引数の数は`$#`で受け取れます．
```shell
if [ $# = 0 ]; then
    引数なしの場合の処理
else
    引数がある場合の処理
fi
```

引数なしの場合は，先程の`echo`コマンドを実行します．

引数がある場合は，引数の数だけ繰り返し処理を行います．
```shell
for i in "$@"
do
    処理
done
```

処理の部分ですが，ファイル名に文字数制限をかけるために，さらに条件分岐を行っています．
```shell
if [ "${#i}" -gt 255 ]; then
    文字数制限に引っかかった場合のエラーメッセージを表示
else
    受け取ったファイル名でipynbファイルを作成する
fi
```

## 完成したファイルの中身
今回作成したシェルスクリプトは以下のようになります．
```shell
#!/bin/sh

if [ $# = 0 ]; then
    echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}' > Untitled.ipynb
else
    for i in "$@"
    do
        if [ "${#i}" -gt 255 ]; then
            echo 'mkipynb: cannot create' "'$i'" ': File name too long'
        else
            echo '{\n "cells": [],\n "metadata": {},\n "nbformat": 4,\n "nbformat_minor": 4\n}' > "$i".ipynb
        fi
    done
fi

exit 0
```

## 実行権限を付与する
次のコマンドで実行権限を付与します．
```terminal
$ chmod +x ~/command/mkipynb
```

## コマンドの使用方法
ファイル名を指定する
```terminal
$ mkipynb foo bar
foo.ipynb  bar.ipynb
```

ファイル名を指定しない
```terminal
$ mkipynb
Untitled.ipynb
```

# おわりに
なぜ，コマンドを作成してまでターミナルからipynbファイルを作成できるようにしたかというと，使い慣れたVSCodeでコーディングしたいからです．

こうしておけば，わざわざJupyter Notebookを起動しなくてもipynbファイルを生成してすぐにVSCodeで開けるようになります．
