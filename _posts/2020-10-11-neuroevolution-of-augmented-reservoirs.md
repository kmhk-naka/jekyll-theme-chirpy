---
title: 【論文読み】Adaptive reservoir computing through evolution and learning
categories: [論文, Survey]
tags: [NEAR, NEAT, リザバー計算, Neuroevolution, 進化計算, 強化学習]
math: true
---

# 論文情報
K. C. Chatzidimitriou and P. A. Mitkas, “Adaptive reservoir computing through evolution and learning,” Neurocomputing, vol. 103, no. 2, pp. 198–209, Mar. 2013.

# なぜ読もうと思ったか？
リザバー計算と進化を組み合わせた論文を探していたから．また，キーワードに強化学習が含まれていたから．

# どんなもの？
様々な機械学習タスクを効率的に処理できる関数近似器を自律的かつ適応的に開発するために，理論とアルゴリズムを組み合わせた新しい方法論であるNEARを提案した．

## NeuroEvolution of augmented reservoirs (NEAR)
NEATをメタ探索アルゴリズムとして用い，進化と学習によって適応したESNの特殊性に合わせたアルゴリズム．

## NeuroEvolution of augmented topologies (NEAT)
人工ニューラルネットワークのトポロジーと重みの進化が可能な最先端のNeuroEvolution手法．
- [Efficient Evolution of Neural Networks](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Efficient+evolution+of+neural+networks+through+complexification&btnG=)
- [Evolving neural networks through augmenting topologies](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.28.5457&rep=rep1&type=pdf)

# 先行研究と比べてどこがすごい？
以下の表にNEARとNEATの類似点と相違点を示す．両者の主な違いは次のとおりである．
- NEARがあらゆる学習アルゴリズムと結合可能であること
- 入力が直接出力に接続されていることによる線形特徴
- 進化するネットワークの種類であること

| Property | NEAT | NEAR |
| --- | --- | --- |
| TWEANN | \\(\sqrt{}\\) | \\(\sqrt{}\\) |
| Non-linear | \\(\sqrt{}\\) | \\(\sqrt{}\\) |
| Linear | \\(\times\\) | \\(\sqrt{}\\) |
| Recursions | \\(\sqrt{}\\) | \\(\sqrt{}\\) |
| Speciation | \\(\sqrt{}\\) (On micro parameters) | \\(\sqrt{}\\) (On macro parameters) |
| Crossover | \\(\sqrt{}\\) (On links—Fittest) | \\(\sqrt{}\\) (On nodes—Adaptive) |
| Open-ended | \\(\sqrt{}\\) | \\(\sqrt{}\\) |
| Incremental | \\(\sqrt{}\\) (On nodes) | \\(\sqrt{}\\) (On nodes) |
| \\(N^{\mathrm{initial}}\\) | \\(0\\) | \\(1\\) |
| Learning | \\(\sqrt{}\\) (Hebbian) | \\(\sqrt{}\\) (Any) |
| Structure | Ad hoc | ESN |


# 技術や手法のキモはどこ？
以下の理論とアルゴリズムを組み合わせた．
- NeuroEvolution of augmented topologies
- Echo State Network
- 学習と進化の結合

# どうやって有効だと検証した？
13種類のテストベッドを用いて，時系列と強化学習の両方の領域で実験を行った．
また，Friedman検定を用いて，[Demšar 2006](https://www.jmlr.org/papers/volume7/demsar06a/demsar06a.pdf)に基づき，NEARとNEATが平均的に同等の性能であるという帰無仮説を検定した．

**時系列**
- Mackey–Glass
- Multiple superimposed oscillator
- Lorentz attractor

**強化学習**
- Mountain car
- Pole balancing
- Server job scheduling
- Ablation study

# 議論はある？
標準的なNEAT手法に対するNEARの優位性は主に次の2つの理由に起因していると考えられる．
- 他の関数近似器を助けることが知られている厳密に線形な特徴が存在しないこと
- ESNのような理論的・実験的に根拠のあるアプローチではなく、アドホックネットワークが開発されていること

# 次に読むべき論文は？
- Dale, Matthew. "Neuroevolution of hierarchical reservoir computers." In Proceedings of the Genetic and Evolutionary Computation Conference, pp. 410-417. 2018.

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| alleviate | 軽減する，緩和する |
| basin | 盆地 |
| bifurcation | 分岐(点) |
| coalition | 合同(ある目的を達成するために複数のグループが結合すること) |
| conjecture | 推測，予想 |
| convention | 慣習，慣例 |
| crude | 自然のままの，粗分析の |
| haystack | 干し草の山 |
| hinder | 妨害する，遅らせる |
| memetic | ミーム学 |
| methodology | 手法, 方法論 |
| offspring | 子，子孫 |
| parsimonious | どけちの，倹約 |
| point out | 〜を指摘する |
| premature | 時期尚早な，早計の |
| shortcoming | 欠点，不十分な点 |
| stagnate | 成長が止まる，沈滞する |
| state-of-the-art | 最先端の |
| superimpose | 重ね(合わせ)る |
| synthesize | 〜を総合的に扱う，〜を合成する |

## メモ
**Neuroevolution**または**Neuro-evolution**は人工ニューラルネットワークの学習に遺伝的アルゴリズムを用いる機械学習の手法である．
