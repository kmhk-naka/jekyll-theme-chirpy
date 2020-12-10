---
title: 【論文読み】A Hypercube-Based Encoding for Evolving Large-Scale Neural Networks
categories: [論文, Survey]
tags: [HyperNEAT, CPPNs]
date: 2020-12-10 15:04:00 +0900
---

# 論文情報
Stanley, Kenneth O., David B. D'Ambrosio, and Jason Gauci. "A hypercube-based encoding for evolving large-scale neural networks." Artificial life 15.2 (2009): 185-212.

**Users Page:** [HyperNEAT User's Page](http://eplex.cs.ucf.edu/hyperNEATpage/)

**Google Scholar:** [A Hypercube-Based Encoding for Evolving Large-Scale Neural Networks](https://scholar.google.com/scholar?oi=gsb05&lookup_url=https%3A%2F%2Fstars.library.ucf.edu%2Fcgi%2Fviewcontent.cgi%3Farticle%3D3177%26context%3Dfacultybib2000&lookup=0&hl=en)

# なぜ読もうと思ったか？
NEATから拡張されたものであり，Atariゲームをプレイするなどの強化学習にも使用されていたから．

# どんなもの？
ジオメトリを利用して大規模なANNを進化させるように設計された，*Hypercube-based NeuroEvolution of Augmenting Topologies*(HyperNEAT)を提案した．

HyperNEATは，直行空間において接続パターンを関数として表現できる*connective compositional pattern-producing networks*(CPPNs)と呼ばれる符号化を採用している．

この論文では，主なアイデアを2つの簡単な例で示し，詳細に分析している．
以下の論文では，HyperNEATがどのように基盤の幾何学的規則性を発見するのか，そしてなぜそれが有用なのかを探っている．

Gauci, Jason, and Kenneth O. Stanley. "Autonomous evolution of topographic regularities in artificial neural networks." Neural computation 22.7 (2010): 1860-1898.

# 先行研究と比べてどこがすごい？
NEATはネットワークを進化させるが，HyperNEATはCPPNsを進化させることで幾何学的規則を明確に学習できるようにした．

# 技術や手法のキモはどこ？
HyperNEATは，ANNsの優れた表現が接続パターンをコンパクトに記述できるはずであるという仮説を立てた表現理論に基づいている．
この種の記述は*encoding*と呼ばれる．
CPPNsと呼ばれるHyperNEATのencodingは，対称性，繰り返し，変化を伴う繰り返しなどの規則性を持つパターンを表すように設計されている．

多くのANNsでは，入力は空間内に幾何学的に配置された状態で存在するセンサの集合から引き出される．
そのような幾何学的配置を認識しない従来のANN学習アルゴリズムとは異なり，接続型CPPN基盤は入力と出力の幾何学的配置を認識しているため，この情報を有利に利用できる．

入力と出力を基板上に適切な構成で配置することで，形状の規則性を*encoding*によって利用できる．
これは，幾何学的な利点の異なる構成を創造的に試してみる余地がある．
例えば，以下の図(Figure 7)は円形ロボットの入力と出力を構成する2つの方法を示しており，それぞれが異なる種類の幾何学的関係を利用する機会を生み出している．

![Figure 7](/assets/img/posts/2020-12-10-Hypercube-Based-Encoding-for-Evolving-Large-Scale-NNs/Figure7.png)

# どうやって有効だと検証した？
次の2つの実験を実施した．

- Visual Discrimination
- Food Gathering

# 議論はある？
[User's Page](http://eplex.cs.ucf.edu/hyperNEATpage/)の「HyperNEAT Methodology FAQ」にいくつかのFAQが載っている．


# 次に読むべき論文は？
HyperNEATを拡張してAtariのゲームプレイに利用している．

Hausknecht, Matthew, et al. "HyperNEAT-GGP: A HyperNEAT-based Atari general game player." Proceedings of the 14th annual conference on Genetic and evolutionary computation. 2012.

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| conquer | 克服する |
| disentagle | 〜のもつれを解く |
| elude | 〜を避ける |
| feat | 手柄，功績 |
| heretofore | これまで，従来 |
| intricate | 複雑な，入り組んだ |
| obfuscate | 曖昧にする，分かりにくくする |
| opaque | 不明瞭な |
| order of magnitude | 〔10進数の〕1桁分，指標 |
| proximal | 隣接した，基部に近い |
| receptive field| 受容野 |
| salient | 顕著な |
| underlying problem | 根本的な問題 |
| vertebrate | 脊椎動物 |
| visual cortex | 視覚野 |

## メモ
