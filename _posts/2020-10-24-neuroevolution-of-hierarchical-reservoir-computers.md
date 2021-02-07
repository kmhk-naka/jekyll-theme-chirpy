---
title: 【論文読み】Neuroevolution of Hierarchical Reservoir Computers
categories: [論文, Survey]
tags: [Reservoir Computing, Neuroevolution]
math: true
---

# 論文情報
Matthew Dale. 2018. Neuroevolution of Hierarchical Reservoir Computers.In GECCO ’18: Genetic and Evolutionary Computation Conference, July 15-19,2018, Kyoto, Japan.ACM, New York, NY, USA, 8 pages. 

DOI: [https://doi.org/10.1145/3205455.3205520](https://doi.org/10.1145/3205455.3205520)

# なぜ読もうと思ったか？
Neuroevolutionとリザバー計算を組み合わせた論文を探していたから．

# どんなもの？
- 進化可能なアーキテクチャ *Reservoir-of-Reservoirs*(RoR) を提案した．
- RoRは，ニューロン(ESNs)のサブネットワークが相互に接続されてリザバーを形成する．(下図参照)

![Reservoir-of-Reservoirs(RoR) architecture](/assets/img/posts/2020-10-24-neuroevolution-of-hierarchical-reservoir-computers/Figure1-1.png)

図は論文(Figure 1)から引用した．

# 先行研究と比べてどこがすごい？
他の階層型ESNとは異なり，次のようなものを含んでいる．
- サブリザバー間の双方向の情報フローのための複数のポイント
- サブリザバー間の適応可能だが疎結合性
- 各サブリザバーのためのローカルハイパーパラメータ
- 適応可能なサブリザバーのサイズ


# 技術や手法のキモはどこ？
仮説として，良いアーキテクチャベースと十分な柔軟性が与えられれば，計算可能な時間内で，与えられたタスクに対して最適な階層，トポロジー，ネットワークの不均一性（または同質性）に向かって進化が収束するというものである．

アーキテクチャを進化させるために，Microbial GA[[link]](https://doi.org/10.1007/978-3-642-21314-4_16)と呼ばれる定常遺伝的アルゴリズムを適用した．


# どうやって有効だと検証した？
RoRとMicrobial GAを評価するために，単一ネットワークと他の階層型ESNを進化させた．
その結果，Microbial GAは他の最適化手法と比較して，単一ネットワークの性能を劇的に向上させることができることを示した．

ベースライン実験として，Microbial GAの性能は，固定サイズのランダムESNと進化したESNを比較することで評価される．

主な調査では，RoRアーキテクチャを進化させ，進化に適応した他の階層型ESNと性能を比較する．

最終的な実験では，進化したRoRは進化していないタスクに再訓練される．

# 議論はある？
以下の文献では，特徴の抽象化を最大化し，多重共線性問題を解消するために、別々の投影層とエンコーディング層が定義されている．各エンコーディング層は低次元表現を生成し，それを次の層に投影する．ここでの進化した解は，最初のリザバーで生成された表現が同じ次元の状態空間に投影される可能性があるため，高次元投影のカスケードを利用できない可能性がある．これが進化した階層で起こるかどうかを理解するためには，さらなる調査が必要である．未使用の突然変異演算子を利用することで，さらなる洞察が得られ，この問題を取り除く可能性がある．

- [Deep-ESN: A Multiple Projection-encoding Hierarchical Reservoir Computing Framework](https://arxiv.org/pdf/1711.05255.pdf)
- [Decouple Adversarial Capacitieswith Dual-Reservoir Network. InInternational Conference on Neural InformationProcessing](https://doi.org/10.1007/978-3-319-70139-4_48)

と書いてあったが，何を言っているのか理解できなかった．

# 次に読むべき論文は？
- Ladosz, Pawel, et al. "Deep Reinforcement Learning with Modulated Hebbian plus Q Network Architecture." arXiv preprint arXiv:1909.09902 (2019).

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| abundant | 豊富な |
| avert | 防ぐ |
| caveat | 注意事項 |
| colinearity | 共線性 |
| counteract | 対抗する |
| cumbersome | 扱いにくい，面倒な |
| deficit | 欠点，弱点，(得)点差 |
| discrepancy | 相違していること |
| feasible | 実現可能な |
| heterogeneous | 異種の |
| homogeneous | 同種の |
| inherently | 本質的に |
| steady-state | 定常状態の |
| subdivided into | 《be 〜》〜に細分される |
| subsume | 組み込む |
| varying degrees of | 〜のさまざまな度合い |

## メモ

| Words and Phrases | 意味 |
| LI-ESN | Leaky integrator ESN |
| M-GA | Microbial GA |
