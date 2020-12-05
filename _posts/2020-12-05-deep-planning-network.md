---
title: 【論文読み】Learning Latent Dynamics for Planning from Pixels
categories: [論文, Survey]
tags: [PlaNet, World Model, 強化学習]
date: 2020-12-05 23:35:00 +0900
---

# 論文情報
Hafner, Danijar, et al. "Learning latent dynamics for planning from pixels." International Conference on Machine Learning. PMLR, 2019.

**Google Scholar:** [Learning Latent Dynamics for Planning from Pixels](https://scholar.google.com/scholar?lr&ie=UTF-8&oe=UTF-8&q=Learning+Latent+Dynamics+for+Planning+from+Pixels+Hafner+Lillicrap+Fischer+Villegas+Ha+Lee+Davidson)

**arXiv:** [https://arxiv.org/pdf/1811.04551.pdf](https://arxiv.org/pdf/1811.04551.pdf)

**CODE:** [https://github.com/google-research/planet](https://github.com/google-research/planet)

**Google AI Blog:** [Introducing PlaNet: A Deep Planning Network for Reinforcement Learning](https://ai.googleblog.com/2019/02/introducing-planet-deep-planning.html)

# なぜ読もうと思ったか？
World modelsを引用していて，ピクセル入力，[planning](#planning)のキーワードがタイトルにあったから．

# どんなもの？
画像から環境のダイナミクスを学習し，潜在空間での高速なオンライン[自動計画](#planning)によって行動選択を行う純粋なモデルベースのエージェントである，Deep Planning Network (PlaNet)を提案した．

# 先行研究と比べてどこがすごい？
長期的な予測を可能にするために，stochastic pathsとdeterministic pathsの両方を持つモデルを設計した．

モデルフリーやハイブリッド強化学習とは対照的に，policy networkやvalue networkを使用しない．

# 技術や手法のキモはどこ？
正確な潜在ダイナミクスモデルを学習するために，

- A Recurrent State Space Model
- A Latent Overshooting Objective

を導入した．

## Recurrent State Space Model (RSSM)
決定論的な要素と確率的な要素の両方を持つ潜在ダイナミクスモデルを用いて，ロバストな計画を立てるために必要なさまざまな可能性のある未来を予測することが出来るようにした．
この実験によって，これら両方の要素が，高いplanningパフォーマンスを実現するために重要であることが示された．

## Latent Overshooting Objective
潜在空間におけるワンステップ予測とマルチステップ予測の一貫性を強制することで，潜在ダイナミクスモデルの標準的な学習目的を，マルチステップ予測を学習するために一般化した．
これにより，長期予測を改善し，どのような潜在シーケンスモデルとも互換性のある，高速で効果的な目標が得られる．

# どうやって有効だと検証した？
連続制御タスクにおいてPlaNetを評価した．

エージェントは観測画像と報酬のみが与えられる．

以下の表は，PlaNetをA3CやD4PGと比較したもの．

PlaNetは，すべてのタスクにおいてA3Cを明らかに上回り，最終的にはD4PGに近い性能を実現している．
また，環境とのインタラクション(エピソード数)は平均で5000%も少なくなっている．

![Table 1](/assets/img/posts/2020-12-05-deep-planning-network/Table1.png)

# 議論はある？
今後の研究の方向性として，固定された動作の繰り返しの代わりに，階層的なモデルを用いて時間的な抽象化を学習することが考えられる．

最終的な性能をさらに向上させるためには，planning horizonを越えて報酬の総和を近似するvalue functionを学習することができる．

さらに，勾配に基づくplanningは，エージェントの計算効率を向上させることができ，再構成無しで表現を学習することは，より多様な視覚的なタスクを解決するのに役立つだろう．

# 次に読むべき論文は？
Warde-Farley, David, et al. "Unsupervised control through non-parametric discriminative rewards." arXiv preprint arXiv:1811.11359 (2018).

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| coincide with | 〜と同時に起こる，〜と一致する |
| for ease of | 〜を楽にするために |
| inference model | 推論モデル |
| log-likelihood | 対数尤度 |
| posterior | 〔位置・順序などが〕後(部)の |
| resemble | 〜のようである，〜と共通点がある |
| traverse | 横断する |

## メモ
### Planning
シミュレーションなどを通して現状認識と問題把握を行い，ポリシーや行動順序などを具体化すること．
例として，知的エージェントや自律型ロボット，無人航空機などが挙げられる．

参考リンク
- [プランニング - AI専門ニュースメディア AINOW](https://ainow.ai/AI%E7%94%A8%E8%AA%9E%E9%9B%86/%E3%83%97%E3%83%A9%E3%83%B3%E3%83%8B%E3%83%B3%E3%82%B0/)
- [自動計画 - Wikipedia](https://ja.wikipedia.org/wiki/%E8%87%AA%E5%8B%95%E8%A8%88%E7%94%BB)

