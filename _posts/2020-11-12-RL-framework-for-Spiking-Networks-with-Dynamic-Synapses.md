---
title: 【論文読み】A Reinforcement Learning Framework for Spiking Networks with Dynamic Synapses
categories: [論文, Survey]
tags: []
date: 2020-11-12 17:45:00 +0900
math: true
---

# 論文情報
El-Laithy, K., & Bogdan, M. (2011). A reinforcement learning framework for spiking networks with dynamic synapses. Computational intelligence and neuroscience, 2011.

**DOI:** [https://doi.org/10.1155/2011/869348](https://doi.org/10.1155/2011/869348)

# なぜ読もうと思ったか？
「Reinforcement Learning」と「Spiking Networks」の2つの単語を見て．


以下，ちゃんと読めていないために中途半端な部分が多数ある．特に数式の説明や議論に関して．


# どんなもの？
[K. El-Laithy and M. Bogdan, “A hebbian-based reinforcement learning framework for spike-timing-dependent synapses,” in Proceedings of the 20th International Conference on Artificial Neural Networks: Part II (ICANN ’10), vol. 6353 of Lecture Notes in Computer Science, pp. 160–169, Springer, 2010.](https://scholar.google.com/scholar_lookup?title=A%20hebbian-based%20reinforcement%20learning%20framework%20for%20spike-timing-dependent%20synapses&author=K.%20El-Laithy%20&author=M.%20Bogdan)
にて紹介したフレームワークのフォローアップ研究を提案した．


# 先行研究と比べてどこがすごい？
生物学的妥当なニューロンとシナプスの表現を明示的に用いて，Hebbian/anti-HebbianとRLの両方のコンセプトを統合したフレームワークである．

この学習アルゴリズムは，シナプスの重みを変化させるのではなく，シナプスのダイナミクスを支配する値(例えば，時定数)に影響を与える．

シミュレーションを行ったSpiking Neural Networkは
- Markram-Tsodyksシナプスモデル
- Leaky integrate-and-fireニューロン
を使用している．

# 技術や手法のキモはどこ？
## Neuronal Model
ニューロンはLeaky integrate-and-fire (LIaF)としてモデル化されており，各ニューロンは，その電圧膜電位\\(V\\)で表現される．

$$ \tau_V \frac{dV(t)}{dt} = V_{\mathrm{rest}} - V(t) + \mathrm{EPSP}(t) \tag{1} $$

- \\(\tau_V\\) は膜時定数: 20 msec
- EPSPはすべてのシナプス前終末から観察された興奮性シナプス後電位の合計

## Synaptic Model (STDP)
これはMarkramらによる短期的なシナプス可塑性のための確立された現象学的モデルである．

時刻tにおけるシナプス前活動電位を仮定すると，抑圧過程は(2)で表すことができる．

$$ \frac{dr(t)}{dt} = \frac{1 - r(t)}{\tau_{\mathrm{rec}}} - u(t) \cdot r(t) \cdot \delta(t - t_i) \tag{2} $$

$$ \frac{du(t)}{dt} = \frac{U_{\mathrm{SE}} - u(t)}{\tau_\mathrm{fac}} + U_{\mathrm{SE}} \cdot (1 - u(t)) \cdot \delta(t - t_i) \tag{3} $$

- \\(\tau_{\mathrm{rec}}\\)はプール回復時定数
- \\(\delta(t - t_i)\\)はディラックのデルタ関数で\\(t_i\\)における入力スパイクを表す
- \\(r\\)は伝達に利用可能な神経伝達物質プールの割合
- \\(u\\)は各スパイクによって利用される\\(r\\)の割合
- \\(U_{\mathrm{SE}}\\)は\\(u\\)のステップ増加数を決定する定数値
- \\(r_{\mathrm{fac}}\\)は緩和時定数

## Reinforcement Learning Framework

![Figure 2(a)](/assets/img/posts/2020-11-12-RL-framework-for-Spiking-Networks-with-Dynamic-Synapses/fig2a.jpg)

図は論文より引用(Figure 2(a))

### Reward Signal

$$ D(f, g) = \frac{1}{\tau_c} \int^{\infty}_{0} [f(t) - g(t)]^2\,dt \tag{4} $$

- \\(\tau_c\\)は指数関数フィルタの時定数で，次のスパイクに対する各スパイクからの影響度を制御する

$$ \mathrm{Rwd} = e^{-\alpha D(F,G)} \tag{5} $$

### Hebbian Update Rule
シナプス作用のダイナミクスは，モデルパラメータを介して表される電気化学的メカニズムの寄与によって支配される．(\\(U_{\mathrm{SE}}\\), \\(\tau_{\mathrm{rec}}\\), \\(\tau_{\mathrm{fac}}\\))
これらのそれぞれのパラメータは\\(m\\)で表される．

$$ \Delta m = \eta \cdot m \cdot \delta_{\mathrm{Rwd}} \tag{6} $$

$$ \delta_{\mathrm{Rwd}} = \mu (\mathrm{Rwd}_{\mathrm{previous}} - \mathrm{Rwd}_{\mathrm{current}}) \tag{7} $$

- \\(\eta\\)は学習率
- \\(\delta_{\mathrm{Rwd}}\\)はTD誤差

# どうやって有効だと検証した？
XOR問題でベンチマークテストをした．

# 議論はある？
これらのネットワークでは，シナプスは重み付け定数として表現されないため，古典的なバックプロパゲーションや\\(\delta\\)ルールを用いてシナプス応答を変更することは適切でない．

シナプス強度の時間発展から見られるように，訓練されていないシナプスの場合には，変動幅だけが収束の影響を受ける．
学習したシナプスでは，シナプス強度は収束時に全く新しい力学的状態に移行する．
このことは，興奮性シナプスと抑制性シナプスの両方で有効である．


# 次に読むべき論文は？
未定．RL関連か進化計算関連で読みやすそうなものを探す．

# その他
## わからなかった英語

| Words and Phrases | 意味 |
| --- | --- |
| action potential | 活動電位 |
| but yet | それでもやはり |
| cleft | cleaveの過去形，裂け目 |
| consistent with | 〜と一致する |
| driven by | 〜によって決定される |
| dynamic regime | 力学的状態 |
| range of fluctuation | 変動幅 |
| postulate | 前提とする，自明ののこととして仮定する |
| potentiation | 相乗効果 |
| presume | 推定する |
| referred to as | 〜と称される |
| uptake of neurotransmitter | 神経伝達物質の取り込み |
| vesicle | 小嚢 |


## メモ
ちゃんと読めておらず，あまり理解できていない．
今までの論文と比べて読みづらく要点がわかりにくかった．
脳神経科学に関する知識不足が理解できなかった原因の1つ．

