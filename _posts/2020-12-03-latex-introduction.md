---
title: 【LaTeX】基本文法のまとめ
categories: [Blogging, Tutorial]
tags: [LaTeX]
math: true
date: 2020-12-03 13:15:00 +0900
---
# LaTeXとは
\\(\mathrm{\TeX}\\)の上で構築されたフリーの文書処理システムです．

数式をよく書くようであれば，WordなどのWYSIWYGソフトよりもかなり楽になります．

また，実体はテキストファイルであるため，Gitでバージョン管理することが可能です．

LaTeXについて詳しくは[LaTeX入門 - TeX Wiki](https://texwiki.texjp.org/?LaTeX%E5%85%A5%E9%96%80)を参照してください．


# この記事を読む上での注意事項
この記事では以下のことを前提として説明しています．

- TeX Liveをインストールしています．
- pLaTeX + dvipdfmxでビルドします．

インストールについては[TeX入手法 - TeX Wiki](https://texwiki.texjp.org/?TeX%E5%85%A5%E6%89%8B%E6%B3%95)を参照してください．

本記事では，ページの読み込みが重くなることを避けるためにビルド結果のPDF内容を記載していません．

本記事は必要に応じて随時更新していきます．

# 基本知識
LaTeXでは，`コマンド`と`環境`によって構成されています．

コマンドは`\title{Hello World}`のように`\`から始まります．

そして，次のように`\begin{}`と`\end{}`で囲まれた部分を環境と呼びます．
```tex
\begin{equation}
f(x) = ax + b
\end{equation}
```

`%`を使用すると，それ以降はコメントアウトされます．
```tex
%%% これは一次関数です．
\begin{equation}
f(x) = ax + b
\end{equation}
```

# 基本文法
## 最低限のテンプレート
`\documentclass[options]{class}`で文書の種類を指定します．

主要なクラスとしては次のようなものがあります．

| class | 概要 |
| --- | --- |
| article | ドキュメントや記事を書くためのもです．|
| report | articleに`\chapter{}`が追加され，報告書の作成に利用されます． |
| book | 書籍を書くためのものです．製本用に奇数ページと偶数ページでレイアウトが異なります． |
| beamer | スライド資料の作成に利用されます． |

そして，`document`環境内に本文を記述します．

```tex
\documentclass[11pt]{jsarticle}

\begin{document}
Hello World
\end{document}
```

環境構築が完了している場合，これをビルドすると「Hello World」と書かれたPDFが生成されます．

`\documentclass{...}`と`\begin{document}`の間の部分を*プリアンブル*と呼び，ここにはパッケージの読み込みやコマンドの定義などを記述します．

## タイトル
`\title{}`コマンドでタイトルを指定します．

`\author{}`で著者名を指定し`\date{}`で日付を指定します．

```tex
\documentclass[11pt]{jsarticle}

\title{タイトル}
\author{著者名}
\date{2020/12/02}

\begin{document}
\maketitle
\end{document}
```

## 見出しと目次
文書の論理構造は以下のようになります．

| コマンド | 概要 |
| --- | --- |
| `\part{}` | 部 |
| `\chapter{}` | 章 |
| `\section{}` | 節 |
| `\subsection{}` | 小節 |
| `\subsubsection{}` | 小々節 |
| `\paragraph{}` | 段落 |

そして，これらの目次を作るには`\tableofcontents`コマンドを使用します．

```tex
\documentclass[11pt]{jsarticle}

\begin{document}
\tableofcontents

\section{セクション}
    ．．．
\subsection{サブセクション}
    ．．．
\end{document}
```

## 箇条書き
### 記号付き箇条書き
`itemize`環境を使用すると，先頭に記号をつけた箇条書きを記せます．
```tex
\begin{itemize}
  \item DCGAN
  \item CycleGAN
  \item WaveGAN
\end{itemize}
```
 
### 番号付き箇条書き
`enumerate`環境を使用すると，先頭に番号をつけた箇条書きを記せます．
```tex
\begin{enumerate}
  \item データの収集
  \item モデルの作成
  \item 学習と予測
\end{enumerate}
```

### 定義型箇条書き
`description`環境を使用すると，先頭に指定したラベルをつけた箇条書きを記せます．
```tex
\begin{description}
  \item[日時] 2020年 12月
  \item[場所] BBQ場
  \item[参加費] 3,000円
\end{description}
```

## 左揃え/中央揃え/右揃え
### 左揃え
`flushleft`環境を使用すると，環境内に記述された文章は左揃えになります．
```tex
\begin{flushleft}
  左揃えになります．
\end{flushleft}
```

### 中央揃え
`center`環境を使用すると，環境内に記述された文章は中央揃えになります．
```tex
\begin{center}
  中央揃えになります．
\end{center}
```

### 右揃え
`flushright`環境を使用すると，環境内に記述された文章は右揃えになります．
```tex
\begin{flushright}
  右揃えになります．
\end{flushright}
```

## 引用
`quote`環境を使用すると，段落の字下げなしで引用を記すことができます．
```tex
ソクラテスの名言
\begin{quote}
  Worthless people live only to eat and drink; people of worth eat and drink only to live.

  価値のない人々は飲食のためだけに生き，価値のある人々は生きるために飲食する．
\end{quote}
```

`quotation`環境を使用すると，段落の字下げありで引用を記すことができます．
```tex
ソクラテスの名言
\begin{quotation}
  To know, is to know that you know nothing. That is the meaning of true knowledge.

  知るということは，あなたが何も知らないということを知ることである．それが本当の知識だ．
\end{quotation}
```

## 脚注
`\footnote[number]{text}`コマンドを使用すると使用すると，ページ下部に脚注を挿入できます．

オプションの`number`は省略可能です．その場合は自動的に番号が振られます．

```tex
CISA\footnote{Certified Information Systems Auditor}によれば，．．．．
```

また，`\footnotemark[number]`と`\footnotetext[number]{text}`を使用して，脚注のテキストを書く場所を分離できます．

```tex
サイドチャネル攻撃\footnotemark[1]を許す脆弱性「PLATYPUS」が判明した．

\footnotetext[1]{暗号装置の動作状況を様々な物理的手段で暗号解読を行う手法の総称}
```

## 数式
### インライン数式
文章中に数式を記述する方法はいくつかありますが，ここでは最もシンプルな`$ $`で囲む方法を紹介します．
```tex
関数$f(x) = Ax(1 - x)$をロジスティック写像といいます．
```

### 番号付き別行立て数式
`equation`環境を使用すると，1行の別行立て番号付き数式を記述できます．
```tex
f(x) = Ax(1 - x)
```

`equation*`環境を使用すると，数式番号が付かない1行の別行立て数式を記述できます．

しかし，数式番号をつけるかどうかを手動で区別するのは面倒なので，`mathtools`パッケージに任せましょう．
`\eqref{label}`コマンドを使用して参照した数式にのみ数式番号がつきます．

```tex
% プリアンブル
\usepackage{mathtools}
\mathtoolsset{showonlyrefs=true}

\begin{document}

\begin{equation}
f(x) = Ax(1 - x) \label{eq:logistic}
\end{equation}

\begin{equation}
\sqrt{x} + \sqrt{y} = \sqrt{a}  (a > 0) \label{eq:parabola}
\end{equation}

1つ目の数式\eqref{eq:logistic}だけに数式番号がつく．

\end{document}
```

### 表記法やシンボル
数式を記述する際によく使用する表記法やシンボルについて記します．
すべてを紹介することができないため，ここに記載のないものは適宜調べてください．

シンボルについては，[List of LaTeX mathematical symbols - OeisWiki](https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols)が参考になると思います．

| 見出し | コマンド | 例 | |
| --- | --- | --- | --- |
| 分数 | `\frac{num}{den}` | \\(\frac{1}{x}\\) | `\frac{1}{x}` |
| 平方根 | `\sqrt[root]{arg}` | \\(\sqrt[2]{10}\\) | `\sqrt[2]{10}` |
| 上付き文字 | `^` (ハット) | \\(a^2\\) | `a^2` |
| 下付き文字 | `_` (アンダースコア) | \\(W_i\\) | `W_i` |
| 総和 | `\sum` | \\(\sum_{i=1}^{N}\\) | `\sum_{i=1}^{N}` |
| 総乗 | `\prod` | \\(\prod_{i=1}^{N}\\) | `\prod_{i=1}^{N}` |
| displaystyle[^displaystyle] | `\displaystyle` | \\(\displaystyle\sum_{i=1}^{N}\\) | `\displaystyle\sum_{i=1}^{N}` |
| ローマン体表記 | `\mathrm{text}` | \\(W_\mathrm{out}\\) | `W_\mathrm{out}` |
| ギリシャ文字 | `\alpha`など | \\(\delta, \Delta\\) | `\delta, \Delta` |
| 微分 | `\prime` | \\(f^\prime, f^{\prime\prime}\\) | `f^\prime, f^{\prime\prime}` |
| 偏微分 | `\partial` | \\(\frac{\partial}{\partial x} f\\) | `\frac{\partial}{\partial x} f` |
| 積分 | `\int` | \\(\int_0^\infty f(x)\,dx\\) | `\int_0^\infty f(x)\,dx` |
| 上線 | `\overline` | \\(\overline{A + B}\\) | `\overline{A + B}` |
| 下線 | `\underline` | \\(\underline{A + B}\\) | `\underline{A + B}` |
| 不等号 | `\geq`, `\leq` | \\(\geq, \leq\\) | `\geq, \leq` |
| ドット | `\dot{}` | \\(\dot{x}\\) | `\dot{x}` |
| ベクトル | `\vec{}` | \\(\vec{x}\\) | `\vec{x}` |

[^displaystyle]: インライン数式では簡略化されて表示されるため，意図的にブロック形式にする方法．[参考](https://a-habakiri.hateblo.jp/entry/2016/12/25/003000)．

#### 行列
行列を表現する方法はいろいろありますが，`array`環境を使用したものが一番汎用性が高いので紹介します．

`\left( \right)`は，囲まれた中身の幅に合わせて伸長します．

`\begin{array}{cols}`の`cols`は，左揃えの`l`と中央揃えの`c`，右揃えの`r`を指定します．

```tex
\begin{equation}
  W = \left(\begin{array}{ccc}
    w_{11} & w_{12} & w_{13}\\
    w_{21} & w_{22} & w_{23}\\
    w_{31} & w_{32} & w_{33}
  \end{array}\right)
\end{equation}
```
$$
  W = \left(\begin{array}{ccc}
    w_{11} & w_{12} & w_{13}\\
    w_{21} & w_{22} & w_{23}\\
    w_{31} & w_{32} & w_{33}\\
  \end{array}\right)
$$

#### 場合分け
場合分けも表現方法はいろいろありますが，`array`環境を使用したものが汎用性が高いでしょう．

`\left\{`について，`{`はエスケープする必要がある点に注意してください．

`\right.`について，場合分けの場合は括弧を閉じる必要はありませんが，`\left`に対して必ず`\right`が必要になります．そのため，`.`を用いて省略しています．

```tex
\begin{equation}
  z = \left\{\begin{array}{ll}
    x & x > 0\\
    0 & x \leq 0
  \end{array}\right.
\end{equation}
```
$$
z = \left\{\begin{array}{ll}
    x & x > 0\\
    0 & x \leq 0
\end{array}\right.
$$

## 図・表
### 図
図を挿入するために，`figure`環境と`\includegraphics[options]{name}`コマンドを使用します．

図には`\caption`を用いてキャプションを設定したり，`\label`を用いてラベルをつけたりできます．

一般的な`figure`環境の例を示します．

```tex
\begin{figure}[h]
  \centering
  \includegraphics[width=0.7\textwidth]{logistic.jpg}
  \caption{ロジスティック写像}
  \label{fig:logistic}
\end{figure}
```

### 表
表を挿入するために，`table`環境と`tabular`環境を使用します．

```tex
\begin{table}[pos]
  \begin{tabular}{cols}

  \end{tabular}
\end{table}
```

表に縦罫線を引くには，`cols`に`|`(パイプ)を記述し，横罫線を引くには`\hline`コマンドを使用します．

```tex
\begin{table}[h]
  \begin{tabular}{|c|l|r|}\hline
    開催回 & 会場 & 参加料\\ \hline
    第1回 & 東京 & 3000円\\ \hline
    第2回 & 大阪 & 2500円\\ \hline
  \end{tabular}
\end{table}
```

表にもキャプションやラベルをつけることができます．

```tex
\begin{table}[h]
  \centering
  \caption{過去の開催情報}
  \label{tbl:info}
  \begin{tabular}{|c|l|r|}\hline
    開催回 & 会場 & 参加料\\ \hline
    第1回 & 東京 & 3000円\\ \hline
    第2回 & 大阪 & 2500円\\ \hline
  \end{tabular}
\end{table}
```

セルは結合することができます．
これについては別記事で説明する予定です．
投稿まで今しばらくお待ちください．

# 今後の投稿予定
これから何本かTeXに関する記事を投稿していきます．
以下は予定している記事の一覧です．

- Beamerを用いたスライド資料作成
- 表組みのセルの結合
- 成果物からソースを見ていく

---
