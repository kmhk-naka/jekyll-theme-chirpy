---
title: 【論文読み】Deep Reinforcement Learning with Modulated Hebbian plus Q Network Architecture
categories: [論文, Survey]
tags: [MOHQA, MOHN, DQN, Hebbian Learning]
date: 2020-11-03 14:33:00 +0900
math: true
---

# 論文情報
Ladosz, Pawel, et al. "Deep Reinforcement Learning with Modulated Hebbian plus Q Network Architecture." arXiv preprint arXiv:1909.09902 (2019).

**link:** [https://arxiv.org/abs/1909.09902](https://arxiv.org/abs/1909.09902)

# なぜ読もうと思ったか？
Modulated Hebbian learningとDQNを組み合わせた新しい学習パラダイムだったので．

# どんなもの？
MOHQAを提案した．

## The modulated Hebbian plus Q network architecture(MOHQA)
このアーキテクチャは次の2要素から成る．
- a standard DQN
- a layer of reward-modulated Hebbian network with neural eligibility traces (MOHN)

このアーキテクチャにおいて，DQNは畳み込み層を介して特徴提供者として機能し，並列MOHNと同等の割合で最終決定に貢献する．

MOHNは，事象の因果関係の連鎖の中で，遅延が変化する観測・行動・報酬のシーケンスを再構成するために採用されている．

MOHNは，TD伝播された状態値ではなく生の報酬シグナルを使用するので，状態が未定義であったり，誤って識別されたりする可能性のあるPOMDP[^1]の交絡において優位性を得ることができる．

![Figure 4](/assets/img/posts/2020-11-03-modulated-Hebbian-network-with-DQN/Figure4.png)  
図は論文より引用．


# 先行研究と比べてどこがすごい？
DQNにとって難しい問題であることが証明されている交絡刺激を伴うPOMDPの問題を，標準的なDQNにMOHNモジュールを追加することで，MOHQAは標準的なDQNよりも高い平均スコアを達成することができた．

このような優位性は，MOHNが重要な決定状態を交絡待ち状態と区別し，状態-行動ペアと報酬の間の時間的ギャップを埋める能力を持つことによって達成された．

重要な決定点を見つけ，状態-行動-報酬のギャップを埋める能力は3つの重要なメカニズムを用いて達成された．
- STDPに着想を得た適格性トレースの新規実装
- Deep RLにおけるmodulated Hebbian Learningの新規使用
- DQNにHebbianベースの構造を統合した新しいDeep RLアーキテクチャ

# 技術や手法のキモはどこ？
## DQN description
DQNは次の式で実装している．

$$ Q(s,a)^* = \max\mathbb{E}[r_t + \gamma r_{t+1} + \gamma^2 r_{t+2} + \ldots |s_t = s, a_t = a, \pi] $$

- \\(r_t\\)は時刻tでの報酬
- \\(s_t\\)は時刻tでの状態
- \\(a_t\\)は時刻tでの行動
- \\(\pi\\)は方策

## STDP-inspired plasticity and neural eligibility traces (MOHN)
シナプス前ニューロンとシナプス後ニューロンの間の修正されたヘブ項\\(\Theta\\)は次の式に従って更新される．

$$
 \Theta_{pre \rightarrow post}(t) = \begin{cases}
    1 & \text{if } v_{pre}(t-1) \cdot v_{post}(t) \text{ is in top } \theta\%,\\
    -1 & \text{if } v_{pre}(t-1) \cdot v_{post}(t) \text{ is in bottom } \theta\%,\\
    0 & \text{otherwise},
 \end{cases}
$$

- \\(v_{pre}\\)と\\(v_{post}\\)はシナプス前・後ニューロンの出力値(MOHNにおける入力・出力層と同等)

神経適格度トレース行列\\(E(t)\\)は\\(\Theta\\)と時間減衰定数\\(\tau_E\\)を基に定式化される．

$$ \dot{E}(t) = -E(t)/\tau_E + \Theta(t) $$

重みの更新のため，トレース行列\\(E(t)\\)に修飾信号を乗じる． また，重みはヘブ更新を含めるため(Miller & MacKay, 1994; Soltoggio &Stanley, 2012)に区間\\([-1,1]\\)にクリップする．

$$ \Delta \mathbf{w}(t) = (r(t) + b) \cdot E(t) $$

最後に，MOHNの出力\\(\mathbf{v}_{mo}\\)は次の式で計算される．

$$ \mathbf{v}_{mo} = \Gamma (\Sigma_j (\mathbf{w} \cdot \mathbf{v}_{mi})) $$

- \\(\Gamma\\)は最大値のindexに\\(1\\)を設定したone-hotベクトルを返す関数

## Integration
DQN headの出力をMOHNの出力と合計するというアイデアは，標準的なDQNが失敗した場合の判断を助けるために，時間的なギャップを超えた因果関係を強調する高レベルのヒントをアーキテクチャ全体に提供することである．
したがって，MOHQAのQ値\\(\mathbf{v}_o\\)は次のように定義される．

$$ \mathbf{v}_o = \mathbf{v}_{mo} + \mathbf{v}_{do} = \mathbf{v}_{mo} + Q(s,A;\Phi^-_i) $$

標準的な手法では行動は次のように選択される．

$$ a_b = \underset{x}{\operatorname{arg\,max}} (\mathbf{v}_o) $$

損失関数は，DQNとMOHNの両方が示すQ値とDQNが示すQ値の間の最良の行動の差を利用している．

$$ L(\Phi) = \mathbf{E}_{(s,a,r,s^\prime)U(D)}\left( r + \gamma \underset{a_b}{\operatorname{max}} \mathbf{v}_o (s^\prime,a_b,\Phi^-_i) - Q(s,a;\Phi_i) \right)^2 $$

# どうやって有効だと検証した？
このアーキテクチャはset of generalized reward-based decision problems that include confounding POMDPsでテストされる．

**A set of confounding POMDPs problems**
- CT-graph Environment (Figure 1)
- MALMO Environment (Figure 2,3)

テストには，
- baseline DQN
- QRDQN+LSTM
- REINFORCE
- A2C
との比較が含まれる．

その結果，MOHQAはconfounding POMDPに対してDQNよりも性能が向上し，より複雑なシナリオでは他のベースラインにも到達できることが示された．

![Figure 1](/assets/img/posts/2020-11-03-modulated-Hebbian-network-with-DQN/Figure1.png)

![Figure 2,3](/assets/img/posts/2020-11-03-modulated-Hebbian-network-with-DQN/Figure2-3.png)

図は論文より引用．

# 議論はある？
この研究では，基本的に単純な交絡POMDPが，特徴空間とポリシーを同時に学習するという困難な問題に洞察を投げかけることを示唆している．

MOHQAは標準的なDQNネットワークよりも複雑であり，追加のハイパーパラメータのチューニングが必要である．

今後の興味深い研究の方向性は，A2Cのような異なる深層強化学習アルゴリズムでMOHQAを実装することである．
これにより，機能が拡張され，安定性が向上し，さらに複雑な交絡POMDPを解くことができるようになるはずである．

# 次に読むべき論文は？
Stanley, Kenneth O., and Risto Miikkulainen. "Efficient evolution of neural network topologies." Proceedings of the 2002 Congress on Evolutionary Computation. CEC'02 (Cat. No. 02TH8600). Vol. 2. IEEE, 2002.

DOI: [https://doi.org/10.1109/CEC.2002.1004508](https://doi.org/10.1109/CEC.2002.1004508)

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| confound | 混乱させる, 悪化させる |
| confounding | 交絡 |
| eligibility | 適任性 |
| eligibility trace | 適格度トレース |
| further test | 追試験 |
| inception | 始まり，発端 |
| noteworthy | 注目に値する |
| priori knowledge | 演繹的知識 |
| proof-of-concept | 概念実証 |


## メモ
### 適格度トレース
強化学習の基本的な仕組みのひとつ．

時間的差異の「ターゲット」とモンテカルロの「リターン」の間で重み付けを行う方法である．
つまり，1段階のTDターゲットを使う代わりに，TD\\((\lambda)\\)ターゲットを使う．
言い換えれば，より良い学習性能を持つようにターゲットを微調整する．

... ということらしい．

詳しくは以下を参照する．
- [Eligibility Traces in Reinforcement Learning](https://towardsdatascience.com/eligibility-traces-in-reinforcement-learning-a6b458c019d6)
- [7. Eligibility Traces](http://incompleteideas.net/book/first/ebook/node72.html)

---

[^1]: Partially observable Markov decision processesl