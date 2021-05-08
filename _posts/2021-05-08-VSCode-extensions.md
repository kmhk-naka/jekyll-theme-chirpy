---
title: VSCodeを使うならとりあえずインストールしてほしい拡張機能と設定変更
categories: [Blogging, Tutorial]
tags: [VSCode, 拡張機能]
date: 2021-05-08 15:00:00 +0900
---

# はじめに
本稿は研究室のメンバーに向けて作成しますが，誰にでも参考になると思うので公開しようと思います．

ビデオ会議などでVSCodeの画面を共有して質問やレビューを受ける時に，特にインストールしておいてほしい拡張機能や設定変更ついて列挙していきます．
拡張機能については，特に嫌いな理由が無ければインストールしておくと助かります．

# 可読性
## テーマ
お気に入りのテーマを見つけて設定しましょう．
[このサイト](https://vscodethemes.com)から探してみるのも良いかもしれません．

個人的なおすすめは次の2つです．

- Dracula Official

![Dracula Official](/assets/img/posts/2021-05-08-VSCode-extensions/Dracula-Official.png){: height="450"}

- GitHub Theme ( `GitHub Dark Dimmed` )

![GitHub Theme](/assets/img/posts/2021-05-08-VSCode-extensions/GitHub-Theme.png){: width="700"}

## indent-rainbow

インデントをレインボーで装飾してくれる拡張機能です．

Pythonのようにインデントが意味を持つプログラミング言語では必須と言っても過言ではないです．

![indent-rainbow](/assets/img/posts/2021-05-08-VSCode-extensions/indent-rainbow.png)

## Bracket Pair Colorizer 2

対応する括弧を色付けして表示してくれます．

コードの解説やレビューを行うときに，「水色の括弧は〜」という風に口頭でも簡単に示すことができて便利です．

![Bracket Pair Colorizer 2](/assets/img/posts/2021-05-08-VSCode-extensions/Bracket-Pair-Colorizer-2.png)

## Rainbow CSV

CSVファイルをレインボーで色分けしてくれます．

人によってはそこまで出番は無いかもしれませんが，入れておいて損は無いと思います．

![Rainbow CSV](/assets/img/posts/2021-05-08-VSCode-extensions/Rainbow-CSV.png){: width="700"}

# Git関連
## Git Graph

リモートも含めたコミット情報をグラフで見やすく表示してくれます．

また，CheckoutやTagの追加などもGUIで簡単に操作できます．

![Git Graph](/assets/img/posts/2021-05-08-VSCode-extensions/Git-Graph.gif){: width="700"}

## GitLens

これはコードの理解を補助してくれる便利で強力な拡張機能です．

具体的には，コミットやファイルの履歴を確認したり `Blame` で誰がいつ変更したのかを確認できます．

ただ，デフォルトの設定だと少し邪魔に感じる部分もあるので以下の設定を無効化しておくと良いと思います．

```json
"gitlens.codeLens.recentChange.enabled": false,
"gitlens.codeLens.authors.enabled": false,
"gitlens.currentLine.enabled": false,
```

# Markdown関連
## Markdown All in One

Markdownファイルを扱うのであれば必須でインストールするべきです．

Table of contentsの自動生成や箇条書きのサポート，表のフォーマットなど様々なサポートをしてくれます．

- List editing

![List editing](/assets/img/posts/2021-05-08-VSCode-extensions/Markdown-All-in-One-List-editing-on-enter-key.gif){: width="40%" .normal}

![List editing](/assets/img/posts/2021-05-08-VSCode-extensions/Markdown-All-in-One-List-editing-tab-backspace.gif){: width="40%" .normal}

![List editing](/assets/img/posts/2021-05-08-VSCode-extensions/Markdown-All-in-One-List-editing-fix-marker.gif){: width="40%" .normal}

- Table formatter

![Table formater](/assets/img/posts/2021-05-08-VSCode-extensions/Markdown-All-in-One-Table-formatter.gif){: width="40%" .normal}

## Markdown Preview Enhanced

その名のとおりマークダウンのプレビュー表示を強化してくれます．

自動スクロール同期や[math typesetting](https://shd101wyy.github.io/markdown-preview-enhanced/#/math)のほかに，[mermaid](https://shd101wyy.github.io/markdown-preview-enhanced/#/diagrams?id=mermaid)や[PlantUML](https://shd101wyy.github.io/markdown-preview-enhanced/#/diagrams?id=plantuml)などのプレビュー機能も備えています．

![Markdown Preview Enhanced](/assets/img/posts/2021-05-08-VSCode-extensions/Markdown-Preview-Enhanced.png){: width="700"}

# 同期／設定／その他
## Live Share

共同編集を可能にする拡張機能です．MSアカウントかGitHubアカウントでサインインしてセッションURLを送ることで招待できます．

ターミナル操作の共有もできます．

ペアプログラミングを行う際には必須となるでしょう．


## Minimapの無効化

行数の多いファイルを編集していて任意の場所にワンクリックで移動したい場合は有効かもしれませんが，みなさんが実験のコードを書く際に利用することはまず無いと思います．

そうすると，ただ表示領域を圧迫している厄介者になってしまうので設定からOFFにしましょう．

```json
"editor.minimap.enabled": false,
```
