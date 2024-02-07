---
alias: 
发布时间: 2020-08-27
出品方: Google
价值: ⭐⭐⭐
环节: 
---
关键词:: #Reco/Debias #ML/因果推断
URL:: [zotero](zotero://open-pdf/0_I62LZPSN/1)

---

# 前言

本文是针对推荐系统中的bias问题，与传统做法有两点不同
- 第一条，方法不同
	- 传统作法是通过喂入dummy feature的方式，比如将position作为特征也喂入模型，然后在预测时将这个position统一赋成0。
	- 本文是遵循inverse propensity weighting（IPW）的做法。“During model training, examples are assigned <span style="color:orange;font-weight:bold">different weights</span> to account for the bias during feedback collection. Intuitively, items that have a <span style="color:orange;font-weight:bold">lower propensity to receive feedback</span> (e.g., those shown at a less visible position in the UI) should be <span style="color:orange;font-weight:bold">assigned higher weights</span>”
* 第二条，范围不同
	* 传统作法往往只关注position带来的bias
	* 本文所讨论的bias不仅仅局限于position，而可以引申到所有attribute，比如device（手机或PC）、feedback source（用户访问到某item，是被动接受推荐，还是主动搜索，还是用户直接访问）

另外，这篇文章还提醒我们注意，让推荐算法debias，从而减少“信息茧房”的风险，
- 改善算法固然是一方面。
- 更重要的是，产品设计上要提供用户通过“非推荐”方式访问item的能力，比如搜索框，比如让用户更容易直接访问到他感兴趣的item，并留下痕迹（比如收藏、历史）。

# Attribute-based Propensity Estimation

## 基本思路
* 是否click，取决于两个hidden variable，i.e., 用户是否examine到那个item，和那个item与user是否相关。
* 用户是否examine到那个item，只和attribute相关。attribute可以包括任何影响用户注意到这个item的因素，比如position, device, feedback source等。
* item与user是否相关，就只与item与user有关，与attribute无关。这也就禁止将attribute作为dummy feature喂入模型。
``` col
![[Pasted image 20220516100519.png]]

![[Pasted image 20220516105046.png]]
```

### IPW

而这个$\theta_{[a]}=P(E=1|[a])$，即在attribute=a下被观测到的概率，就是所谓的Inverse Propensity Weight (IPW)。而样本的权重=$\frac{1}{\theta_{[a]}}$，即越难被观测到，其权重就应该越大。
❓ 但是，<span style="color:crimson;font-weight:bold">是否应该对正负样本都采用相同的权重规则？</span>
* 如果是正样本，这个样本越难被观测到，但是还是点击了，肯定能够说明<u,i>之间强烈的匹配，应该增加其权重
* 如果是负样本，**样本被观察到的难度越高，说明这个click=0越不置信，反而应该降低其权重，起码不应该再增加其权重了**。

或者反过来问一个问题，<span style="color:crimson;font-weight:bold">被观察到的概率越高，其权重就应该越低</span>❓❓❓
- 如果click=1，自然是
- 但是如果click=0，这才是真正反直觉的，权重应该越高才对

## 使用EM估算参数
- 剩下的问题就是如何得到$\theta_{[a]}=P(E=1|[a])$?
	- 通过实验统计是一种方法，但是<span style="color:orange;font-weight:bold">这种实验必须完全抛弃推荐算法</span>
		- 每种attribute展示的item都是随机的。
		- 这种纯随机实验，既伤害用户体验，在海量item下又是不可能的。
	- 所以只能从现有日志数据中估计出。

### 类比GMM

EM算法推导起来很复杂，但是最终的结果还是非常直观的。以Gaussian Mixture Model为例
* x代表观察到的样本
* z代表x所属的类别，是hidden variable
* $\phi_j$是第j个类别的占比，即第j个类别的占比占总样本的比例
* $\mu_j,\Sigma_j$是第j个类别的gaussian分布的中心与variance
```` col

``` col-md

#### Expectation Step
Expectation就是，基于已经观测到的样本“x”和上一轮的参数，估计hidden variable “z”的分布，即$w_j^{(i)}$是指第i个样本属于第j个类别的可能性
![[Pasted image 20220516110003.png]]
```

``` col-md
#### Maximization Step
通过上一步，**各样本x的hidden variable的分布就变成已知**。基于已知的hidden variable的分布，基于MLE估算所有参数（e.g., 各类别的占比、每个类别的gaussian的center与variance）
![[Pasted image 20220516110022.png |350]]
```

````


### EM估算方法
```` col

``` col-md
#### Expectation Step
每个样本，基于观测到的“点击与否”，估计hidden variable (i.e, **是否观测到+是否相关**)的概率
![[Pasted image 20220516142221.png |300]]

以上结果都是非常直观的，
* 比如第一个，如果clicked，自然是又被观测到了，而且还相关
* 比如第二个，C=0作为分母，其概率是$1-\theta_{[a]}^t\gamma_{i,u}^t$；E=1的概率是$\theta_{[a]}^t$；R=0的概率是$1-\gamma_{i,u}^t$

注意，**除了列举出来的概率，其他概率都为0**
* e.g., p(E=0,R=0|C=1)=0，因为既然已经点击了，就不可能即没有观察到，又不相关
* e.g., p(E=1,R=1|C=0)=0，因为既然又相关，又被观测到，不可能不点击

```

``` col-md
#### Maximization Step
在上一步已经将各hidden variable的分布估计出来的情况下，我们就可以估算各参数的分布。以$\theta_{[a]}$为例，直观理解起来，就是在所有attribute=a的样本中，“观测到=1”的样本所占比例。
* 分母是所有attribute=a的样本的个数
* 分子又为c=1和c=0两种情况。
* c=1时，既然已经点击，自然就已经观测到了，分子+1
* c=0时，分子就不能再+1，而是加上一个soft label，即p(e=1|c=0)。而`p(e=1|c=0)=p(e=1,r=1|c=0)+p(e=1,r=0|c=0)`。
  * 前者p(e=1,r=1|c=0)=0，因为不可能既观察到，又相关，还不点击
  * 后者p(e=1,r=0|c=0)，如expectation step所推导的一样，=$\frac{\theta_{[a]}(1-\gamma_{i,u}^t)}{1-\theta_{[a]}^t\gamma_{i,u}^t}$


所以对$\theta_{[a]}$的估计可以为
![[Pasted image 20220516143127.png |350]]


同理，可以推导出对“<u,i>匹配度”$\gamma_{u,i}$的估计
![[Pasted image 20220516143147.png |350]]

```

````

以上EM两个步骤反复迭代多次，收敛之后的$\theta_{[a]}$的倒数$\frac{1}{\theta_{[a]}}$，将会用于给[样本加权]()，训练一个unbiased model。

### Regressing <u,i> relevance

在maximization step中计算$\theta_{[a]}^{t+1}$倒没什么问题，已经attribute中的具体取值是有限的，用sample mean来估计也是具备高可信度的。问题出在估算$\gamma_{u,i}$上，毕竟我们不可能为每个<u,i>来统计，即使能够统计，也由于某一对<u,i>的数据太稀疏，导致公式中的sample mean的结果是极其不置信的。

解决方法就是训练一个简单模型，输入user feature和item feature计算出二者之间的相关性，取代对每一对<u,i>的统计

- 通过E-step，我们已经知道P(r=1|c)
	- p(r=1|c=1)=1，因为根据假设，点击过的一定是相关的
	- $p(r=1|c=0)=p(e=1,r=1|c=0)+p(e=0,r=1|c=0)$=$0+\frac{(1-\theta_{[a]})\gamma_{u,i}^t}{1-\theta_{[a]}^t\gamma_{i,u}^t}$=$\frac{(1-\theta_{[a]})\gamma_{u,i}^t}{1-\theta_{[a]}^t\gamma_{i,u}^t}$
	- 以上公式中用到的$\gamma_{i,u}^t$，来自上一个模型，是已知的
- 根据以上得到的P(r=1|c)抽样label(i.e., relevant=0/1)，以user feature/item feature为输入，训练一个简单的binary classification model (i.e., depth=3的GBDT)作为$\gamma_{i,u}^{t+1}$  
	- 训练$\gamma_{i,u}^{t+1}$的时候，可以拿$\gamma_{i,u}^{t}$作为warm-start


💡 注意，这个步骤的模型与第二阶段模型，二者有区别，不要混淆
* 这个模型是用来预测<u,i>之间的相关性，而在第二阶段，我们是通过re-weight方式训练一个CTR预估模型。由于有“是否观察到”因素的影响，二者的目标并不相同
* 这个相关性模型的label是通过某个expectation step中预估p(r=1)采样得到的，而第二阶段模型的label（点击与否）来自用户真实反馈
* 第二阶段模型只训练一遍，而这个相关性模型需要在每个maximization step都训练出一个新模型（但是可以借助上次maximization step中的模型warm-start）

## Use Case
- position bias, attribute就只有position 'k'
- platform bias, attribute是platform 'p'和position 'k'的组合
- external feedback bias，也就是用户不是通过推荐算法与item交互的情况。方法是为这些feedback设置一个特殊的位置“ke”
	- 但是有一个工程实现上的难点，就是无论是在EM估算$\gamma_{i,u}^{t+1}$，还是在第二阶段训练unbiased模型时，都需要user feature和item feature。而对于这种non-recommender external interaction，推荐引擎是不可dump特征的。论文中也没有提出明确的解决方案，就是说要监控这部分的coverage。

# 评估

## MRR
![[Pasted image 20220516150041.png |200]]
* N是"评估样本"的个数
* $rank_i$=the minimum rank of the clicked items for the 𝑖-th list

## WMRR
![[Pasted image 20220516150105.png |300]]
* $w_i$ is the learned IPW weight for minimum-rank clicked item in i-th list.
* WMRR gives a higher reward (assuming 𝑤 > 1) to models that improve the rank of hard-to-find positive items during the data collection phrase
* Experiments show offline WMRR is correlated with online metrics better than MRR.