---
title: 【論文読み】Supervised and Evolutionary Learning of Echo State Networks
categories: [論文, Survey]
tags: [進化的アルゴリズム，ESN]
math: true
date: 2020-12-2 00:58:00 +0900
---

# 論文情報
Jiang, Fei, Hugues Berry, and Marc Schoenauer. "Supervised and evolutionary learning of echo state networks." International Conference on Parallel Problem Solving From Nature. Springer, Berlin, Heidelberg, 2008.

**DOI:** [https://doi.org/10.1007/978-3-540-87700-4\_22](https://doi.org/10.1007/978-3-540-87700-4\_22)

**Google Scholar:** [Supervised and Evolutionary Learning of Echo State Networks](https://scholar.google.com/scholar?oi=gsb05&lookup_url=https%3A%2F%2Fhal.inria.fr%2Finria-00337235%2Fdocument&lookup=0&hl=en)

# なぜ読もうと思ったか？
Reservoir-of-Reservoirの論文中に引用されていたため．


# どんなもの？
モータ制御タスクに対してEAとESNの組み合わせの実現可能性を示した最初の研究である．


# 先行研究と比べてどこがすごい？
これまでの論文では，進化的最適化をリザバー重みや出力重みに限定していたが，追加のESNを最適化することが可能であることを示した．


# 技術や手法のキモはどこ？
CMA-ESとESNを組み合わせて，制御タスク(double pole balancing)問題に使用した．


# どうやって有効だと検証した？
## Supervised Learning of ESN
次の3つのバリエーションのESN進化的最適化を比較した．

* optimizing the output weights only, denoted *Std*
* optimizing the output weights *plus* the spectral radius, denoted *Rho*
* optimizing the sigmoidal slopes only, denoted *Slopes*

![Fig.2.](/assets/img/posts/2020-12-02-supervised-and-evolutionary-learning-of-ESN/Figure2.jpg)

図は論文中(Fig.2.)より引用．

この結果から，reservoir slopes(*Slopes*)のみを最適化することで，オリジナルのESN学習法と同様の精度が得られることが示された．

しかし，reservoir sizeが小さい場合(N=30)では，reservoir neuron slopesを最適化することで，標準的な手順よりもさらに優れたfitnessが得られた．
ただし，これにはコストがかかり，このような低いfitness値を見つけられる実行は非常に少ないため，100倍近くの評価が必要になる．

最後に，Fig.2.は探索空間を増やしても精度が向上しないことを証明している．

## Reinforcement Learning of ESN
速度情報なしのdouble pole balancing問題でベンチマークテストを実施した．

ヘビーな計算コストを避けるために，double pole balancing問題に対処する進化論的文献の以前の研究では，簡略化されたfitnessが使用されている．

$$F_{\mathrm{cheap}} = 10^{-4}t + 0.9 f_{\mathrm{stable}}$$

$$f_{\mathrm{stable}} = \left\{\begin{array}{lcl}
        0 & \, & \text{if}\, t < 100\\
        \frac{0.75}{\sum^{t}_{i=t-100}(|x(i) + |\dot{x}(i)| + |\theta_1(i)| + |\dot{\theta}_1(i)|)} & \, & \text{otherwise}
\end{array}\right.
$$

\\(f_{\mathrm{stable}}\\)は過去100タイムステップの間のカートの安定性を定量化する．

![Table 1.](/assets/img/posts/2020-12-02-supervised-and-evolutionary-learning-of-ESN/Table1.jpg)

表は論文中(Table 1.)より引用．

\\(F_{\mathrm{cheap}}\\)を使用した場合，最初の印象的な結果は，*Std*の性能が非常に低いことである．
これらのケースでは，3回の汎化テスト(*Generalization*)に合格した実行の\\(7\%\\)未満である．

ネットワークの汎化能力を考慮した新しいfitnessを使用した場合，結果は改善される．
例えば，*Rho*はほとんど常に解を見つける．
*Std*でさえ，\\(F_{\mathrm{cheap}}\\)を使用した場合の結果よりも大幅に改善している．

### Spectral Radius
ESNのパイオニアたちは，スペクトル半径の上限がESNの使用を成功させるために重要であることを常に主張してきたが，異なるスペクトル半径を持つ両方の*Std*の結果はこれを裏付ける．
しかし，最も注目すべき事実は，すべての設定において，スペクトル半径を明示的に最適化する*Rho*がほぼ常に最良の結果をもたらしていることである．

### Reservoir Topologies
Double pole balancing問題で得られた結果は，接続密度が同じ値の場合，リザバー内の接続の異なる実現の間で大きく異なることがわかった．
実際，性能の低い手法の場合，すべての成功した実行は，多くの場合，少数の初期リザバートポロジーに起因しており，大多数の初期リザバートポロジーは1つの成功すら生成できていない．
教師あり学習の文脈で指摘された違いと合わせて，リザバーのトポロジーが重要であることを明確に示している．その理由とこの事実をどのように利用するかは，今後の研究に委ねられている．


# 議論はある？
少なくともここで実験した小さなサイズについては，リザバーのトポロジーに高い結果の依存性があるようだ．
したがって，ここで示された結果は，ESNが今日の神経進化の両極端(以下の2点)の間のどこに立っているかという疑問に十分答えていない．
- fully RNNの重みの進化的最適化
- 与えられたタスクのために高効率のNNのトポロジーを進化させる慎重に作られた発達システム

この問題に確実に対処するためには，より信頼性の高いテスト問題とより大きなreservoir sizeを用いた更なる実験が必要である．

# 次に読むべき論文は？
Hyper-NEATの論文か，ESNとRL関連の論文を読む．


# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| Albeit | 〜にもかかわらず |
| arbitrary | 恣意的な |
| feasibility of | 〜の実現可能性 |
| lieu | 〜の代わりに |
| preliminary results | 予備段階の結果，中間集計 |
| prescribe | 規定する |
| single degree of freedom | 単一自由度 |
| striking | 印象的な，著しい |
| thereafter | それ以来 |
| thorough | 徹底的な |
| undergo a test | テストを受ける |


## メモ
*reservoir slope*がどういうものなのか分からなかった．
