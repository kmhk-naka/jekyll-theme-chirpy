---
title: 【論文読み】Evolving  Neural Networks through Augmenting Topologies
categories: [論文, Survey]
tags: [NEAT, Neuroevolution, 強化学習]
date: 2020-11-14 22:28:00 +0900
math: true
---

# 論文情報
Stanley KO, Miikkulainen R. Evolving neural networks through augmenting topologies. Evolutionary computation. 2002 Jun;10(2):99-127.

**DOI:** [https://doi.org/10.1162/106365602320169811](https://doi.org/10.1162/106365602320169811)


# なぜ読もうと思ったか？
Neuroevolution手法のなかで特に有名で，様々な応用が提案されているため読もうと思った．


# どんなもの？
NeuroEvolution of Augmenting Topologies (NEAT)を提案した．

NEATはニューラルネットワークのトポロジーを重みとともに進化せることで，困難な強化学習ベンチマークテストにおいて，従来のトポロジーを固定した手法を上回る成果をあげた．

TWEANNsにおける以下の3つの疑問に対して解決策を示した．
- 異種のトポロジーが意味のある形でクロスオーバーすることを可能にする遺伝的表現はあるのだろうか？
- 最適化のために数世代を必要とするトポロジカルなイノベーションは，どのようにして集団から早々に消えてしまわないように保護することができるのだろうか？
- 複雑さを測定するための特別に考案された適合度関数を必要とせずに，進化全体を通してトポロジーを最小化するにはどうすればよいか？

> - Is there a genetic representation that allows disparate topologies to cross over in a meaningful way?
> - How can topological innovation that needs a few generations to be optimized be protected so that it does not disappear from the population prematurely? 
> - How can topologies be minimized throughout evolution without the need for a specially contrived fitness function that measures complexity?

# 先行研究と比べてどこがすごい？
効率の向上は次の3点によるものと考えられる．
- historical markings
- speciation
- incremental growth from minimal structure


# 技術や手法のキモはどこ？
## Genetic Encoding
NEATの genetic encoding スキームは，交配中に2つのゲノムがクロスオーバーした際に，対応する遺伝子が用意に並ぶように設計されている．

NEATにおける突然変異は，接続重みとネットワーク構造の両方を変化させることができる．

構造的な突然変異は以下の図に示すように，2つの方法で発生する．

- *add connection*突然変異では，これまで接続されていなかった2つのノードを接続する新しい接続遺伝子が1つ追加される
- *add node*突然変異では，既存の接続が分割され，古い接続があった場所に新しいノードが配置される

![structual mutation](/assets/img/posts/2020-11-14-neuroevolution-of-augmenting-topologies/figure3.png)

図は論文より引用(Figure 3)．


## Tracking Genes through Historical Markings
新しい遺伝子が(構造的突然変異によって)現れるたびに，グローバルなイノベーション番号がインクリメントされ，その遺伝子に割り当てられる．
また，現在の世代で発生したイノベーション番号のリストを保持しておくことで，同じ構造が独立した突然変異によって同一世代に複数回発生した場合，それぞれの同一の突然変異には同じイノベーション番号が割り当てられることを保証できる．

これにより，システムは現在，どの遺伝子がどの遺伝子と一致しているかを正確に把握している．
クロスオーバーの際に，同じイノベーション番号の遺伝子が両方のゲノムに並んでいる．
これらの遺伝子をマッチング遺伝子と呼ぶ．
一致しない遺伝子は，もう一方の親のイノベーション番号の範囲内で発生しているか，範囲外で発生しているかによって，*excess*遺伝子か*disjoint*遺伝子のどちらかになる．

このようにして，historical markings により，NEATは高価なトポロジー解析を必要とせずに，線形ゲノムを使用してクロスオーバーを実行することができる．


## Protecting Innovation through Speciation
NEATにおける異なる構造の compatibility distance \\(\delta\\) を，*excess*遺伝子(\\(E\\))と*disjoint*遺伝子(\\(D\\))の数と，障害遺伝子を含む一致遺伝子の平均重量差(\\(\overline{W}\\))の単純な線形の組み合わせとして測定することができる．

$$ \delta = \frac{c_1 E}{N} + \frac{c_2 D}{N} + c_3 \cdot \overline{W} $$

距離尺度\\(\delta\\)は compatibility threshold \\(\delta_t\\) を用いて speciate を可能にする．

organism \\(i\\) のための調整された適合度 \\(f^{\prime}_i\\) は，個体群における他のすべての organism \\(j\\) からの距離 \\(\delta\\) によって計算される．

$$ f^{\prime}_i = \frac{f_i}{\Sigma^n_{j=1} s \mathbf{h}(\delta (i,j))} $$

## Minimizing Dimensionality through Incremental Growth from Minimal Structure
TWEANNsの場合，最初から多様性を導入するためにランダムなトポロジの初期個体群から開始している．

NEATでは対照的に，隠れノードが1つもない一様な個体群から開始することで，最小次元空間への探索にバイアスをかけている．
構造的な変異が起こるたびに新しい構造が挿入され，適合度評価によって有用な構造だけが生き残っていく．

初期個体群が最小であるため，探索空間の次元性が最小化され，TWEANNsや固定トポロジー進化計算システムよりも常に少ない次元で探索している．

# どうやって有効だと検証した？
XOR問題やPole Balancingをベンチマークタスクとして行った．

さらに，各構成要素がシステム全体として，またはお互いに必要であることを証明するアブレーションスタディを通して検証した．


# 議論はある？
実験の結果，[ESP](#enforced-subpopulations-esp)とNEATの差は，最も困難なPole Balancingタスクにおいて有意に大きいことが示された．
この結果は，多様なトポロジーを進化させることが，他の方法では行き詰まる可能性が高い問題に特に適していることを示唆している．

NEATは常に構造を追加することができるため，ネットワークの現在の重みが適合度空間の局所最適を表していたとしても，必ずしも嵌まるわけではない．


# 次に読むべき論文は？
Stanley KO, D'Ambrosio DB, Gauci J. A hypercube-based encoding for evolving large-scale neural networks. Artificial life. 2009 Apr;15(2):185-212.

**DOI:** [https://doi.org/10.1162/artl.2009.15.2.15202](https://doi.org/10.1162/artl.2009.15.2.15202)

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| assumption | 仮定 |
| as opposed to | 〜とは対照的に |
| chronology | 〔時間順の〕出来事の配列 |
| contrive | 考案する |
| disparate | 完全に異なる |
| encompass | 網羅する |
| extraneous | 無関係の |
| finite limit | 有限極限 |
| gradually | 徐々に |
| inevitable | 当然の，必然的な |
| lead to a loss of | 〜の損失につながる |
| prematurely | 通常より早く |
| persuasive  argument | 説得力のある論拠 |
| perturbation | 摂動 |
| prevailing | 広まっている，一般的な |
| put forward | 提案する |
| take advantage of | 〜をうまく[巧みに]利用する |
| thus far | これまでに |
| to date | 今まで |
| unbounded | 無制限の |

## メモ
### Ablation study
各構成要素を部分部分で見ていくような実験のこと

### perturbation (摂動)
一般に撹乱，変化のことをいう．

### Enforced Subpopulations (ESP)
ニューロンの集団は部分集団に分離されていいる．
ネットワークは，各部分集団から1つのニューロンをランダムに選択することで形成される．
このようにして，ニューロンは自分が参加している各ネットワークの適合度を加算することで，適合度スコアを蓄積していく．
各部分集団内で最も優れたニューロンは，新しいニューロンを形成するために交配される．

[参考](https://www.researchgate.net/figure/Enforced-SubPopulations-ESP-The-population-of-neurons-is-segregated-into_fig1_248554235)
