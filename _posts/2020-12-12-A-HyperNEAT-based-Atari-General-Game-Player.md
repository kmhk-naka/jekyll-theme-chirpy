---
title: 【論文読み】HyperNEAT-GGP&#58; A HyperNEAT-based Atari General Game Player
categories: [論文, Survey]
tags: [HyperNEAT, HyperNEAT-GGP, Atari]
math: true
date: 2020-12-12 20:21:00 +0900
---

# 論文情報
Hausknecht, Matthew, et al. "HyperNEAT-GGP: A HyperNEAT-based Atari general game player." Proceedings of the 14th annual conference on Genetic and evolutionary computation. 2012.

**DOI:** [https://doi.org/10.1145/2330163.2330195](https://doi.org/10.1145/2330163.2330195)

**Google Scholar:** [HyperNEAT-GGP: A HyperNEAT-based Atari general game player](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=HyperNEAT-GGP%3A+A+HyperNEAT-based+Atari+general+game+player)

# なぜ読もうと思ったか？
Google Scholarで「HyperNEAT」と検索するとトップに出てきたので．

# どんなもの？
Visual Processingアーキテクチャによって，Atariゲームの学習にHyperNEATを利用可能にした．
(これが最初かどうかは知らない)

関連研究(Master's Thesis)として以下のものがある．

**Google Scholar:** [Game-independent AI agents for playing atari 2600 console games](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Game-independent+AI+agents+for+playing+atari+2600+console+games)

# 先行研究と比べてどこがすごい？
どこがすごいのかはわからなかった．

# 技術や手法のキモはどこ？
ゲーム画面の生のピクセルを以下の図のように抽象化してHyperNEATに入力するようにした．

![Figure3](/assets/img/posts/2020-12-12-A-HyperNEAT-based-Atari-General-Game-Player/Figure3.png)

以下の疑似コードは自己識別の手順を示している．

![Pseudocode](/assets/img/posts/2020-12-12-A-HyperNEAT-based-Atari-General-Game-Player/Algorithm1.png)

入力として，1-3行目では，アルゴリズムは，現在のゲームに適用可能なジョイスティックとボタンのアクションのセットと，現在のフレームで検出されたblobのリスト，エージェントによって取られた行動の履歴にアクセスできる．
さらに，5行目は，すべてのblobの \\((x,y)\\) 速度履歴 \\(vHist\\) へのアクセスを仮定している．
次に，blob \\(b\\)の速度履歴のエントロピーを次の式に従って計算する．

$$
H(X) = - \sum^n_{i=1} p(x_i) * \ln(p(x_i))
$$

エントロピーを計算した後，次のアクションを個別に調査し，8行目で*b*の選択的速度履歴 \\(vHist_{(b \| a)}\\) を作成する．
9行目では，\\(b\\)の選択的速度履歴に基づいてエントロピーが計算される．
この選択的エントロピー\\(H_{(b\|a)}\\)は，与えられたアクションが確実にblobを特定の方向に移動させる場合，低くなければならない．

最後に，11行目で，blobの全速度履歴エントロピーからblobの選択速度エントロピーの重み付け周波数の合計を差し引くことで情報利得を計算する．

# どうやって有効だと検証した？
*Freeway*と*Asterix*の2つのAtariゲームで実験をした．

学習プロセスにおける個体群の適合値の平均とチャンピオンは以下の図で確認できる．(Figure5: *Freeway*, Figure6: *Asterix*)

![Figure 5](/assets/img/posts/2020-12-12-A-HyperNEAT-based-Atari-General-Game-Player/Figure5-6.png)

# 議論はある？
Algorithm1は一般的に自己blobを識別することに成功しているが，ゲームのダイナミクスによっては，アクションの結果が同じような速度になるという仮定が崩れてしまうことがある．
例えば，*freeway*では，車と衝突した後，プレイヤーから制御が奪われ，エージェントがどのアクションを実行しているかにかかわらず，鶏が数フレームのあいだ下に移動してしまう．
この一時的な制御の欠如は，不規則な選択的速度履歴と一時的に自己の識別を悪くする結果となる．

# 次に読むべき論文は？
Gauci, Jason, and Kenneth O. Stanley. "Autonomous evolution of topographic regularities in artificial neural networks." Neural computation 22.7 (2010): 1860-1898.

**Google Scholar:** [Autonomous evolution of topographic regularities in artificial neural networks](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Autonomous+evolution+of+topographic+regularities+in+artificial+neural+networks)

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| adjacent | 隣接した |
| blob | おぼろげなもの |
| daunting task | 困難な仕事 |
| depiction | 描写 |
| devoid of | 〜を欠いている |
| inadvertently | 不注意に |
| ingenuity | 創造力，巧妙なアイデア |
| insurmountable | 克服できない |
| impart | 〜を授ける |
| maneuver | {名}作戦行動，{他動}操作する |
| novice | 未熟者 |
| sinusoid | 正弦曲線 【同】sine curve |
| spurious | 偽の，誤った |


## メモ
