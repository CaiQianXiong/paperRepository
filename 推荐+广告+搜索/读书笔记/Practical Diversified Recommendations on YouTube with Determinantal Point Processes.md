---
alias: DPP
发布时间: 2018-10-17
出品方: Google
价值: ⭐⭐⭐⭐
---

环节:: 重排
关键词:: #Reco/多样性
URL:: [zotero](zotero://open-pdf/0_IA8W6N48/1)

---

# [Practical Diversified Recommendations on YouTube with Determinantal Point Processes](https://jgillenw.com/cikm2018.pdf)

## 概述

- *用DPP来描述用户点击的过程，不是理论证明的，而是我们选择的结果*
	- Then we *assume* that a user u’s behavior is modeled by a DPP with probability distribution P in the following manner: Y ∼ Pu
	- the set of videos interacted with, Y , represents a draw from the probability distribution defined by a user-specific DPP.
	- 然后，这个user-specific DPP can be compactly parameterized by a single N × N positive semidefinite kernel matrix

- 如何构建这个L，有多种方法
	- 一种是[[Practical Diversified Recommendations on YouTube with Determinantal Point Processes#构建普通的Gram矩阵 |构建普通的Gram矩阵]]
	- 一种是根据后验点击数据，把最佳构建方式构建出来

---


![image.png | 500](assets/image-20220114103122-omqwpw4.png)
* DPP重排模块是在精排（pointwise video scorer）之后，而在打散(policy layer)之前
* 需要两个输入
  * item本身的质量，由精排模块提供
  * item embedding，用于计算item之间的相似性，可以由双塔模型提供
  * 本算法的好处在于，DPP依赖于粗排+精排的输出，但是并不与那两个模块耦合，重排模块可以独立发展


## DPP基础

* A point process P on a set S = {1, 2, . . . , N } is a probability distribution on the powerset of S (the set of all subsets of S). That is, ∀S ⊆ S, P assigns some probability P(S), and $\sum_{S} P(S) = 1$.
* DPP是一种特殊的point process，根据DPP采样得到的点，有两个特殊性质
  * P(Y)可以由一个特殊矩阵L中由Y指定的子矩阵的determinant计算得出，从而将复杂的概率运算转为化矩阵运算
  * L是需要如此构造出来的特殊矩阵
    * $L_{ii}$ measures the quality of item i
    * $L_{ij}$ is a scaled measurement of the similarity between items i and j.
  * 假设只采样2个点，即|Y|=2, 那么采样到Y的概率=det(![image.png | 100](assets/image-20220114102355-df7l5j6.png))=$L_{11}L_{22}-L_{12}L_{21}$  
    * **与item本身的质量正相关，与item之间的相似性负相关，正是我们所需要的**
* 因此，我们重排的目标就是, finding the set $max_{S:|S |=k} P(S)$ is one way of selecting a high-quality and diverse subset of k items from a larger pool of N items.


所以DPP应用于重排的前提是，**一次feed提供N items，用户看了哪些（用Y表示），是一个DPP过程**，即用户看的哪些与自己相关，同时与同一刷中上一个看过的item迥异的。**That is, the set of videos interacted with, Y, represents a draw from the probability distribution defined by a user-specific DPP.**

* 既然如此，我们就可以定义Y（一刷N个中用户选择的那些）的概率
* 先定义半正定矩阵L。L可以是最普通的Gram Matrix. $L_{ij}=g(i)^T g(j)$，g(i)表示第i个item的特征向量。也可以是由考虑了item itself quality + item~item similarity更复杂的Kernel Matrix
* $L_Y$是由Y指定的行与列组成的小的sub matrix
* P(Y)（一刷N个中用户点击哪些的概率）=$\frac{det(L_Y)}{\sum_{Y^{'} \in S}det(L_{Y^{'}})}$，再根据matrix的良好性质，可以简化为$P(Y)=\frac{det(L_Y)}{det(L+I)}$

解![image.png | 100](assets/image-20220114114649-ujovoqd.png)是NP-hard，有standard greedy algorithm for submodular maximization的近似算法，

* The greedy algorithm starts from Y = ∅ (the empty set),
* then runs k iterations, adding one video to Y on each iteration.（插入k的顺序也就是这个小窗口呈现给用户的顺序）
* The chosen video in iteration i is the video v that produces the largest determinant value when added to the current chosen set ![image.png | 200](assets/image-20220114114809-1m2fbc9.png)

## 构建普通的Gram矩阵

- 构造一种特殊的Gram Kernel Matrix
	- 是==heuristic==的方式构建出来的
	- 合理假设：用户喜欢相关性高的，不喜欢与之前重复的
	- 根据以上假设，才构建出一个对象阵都是rank prediction score，对角性上是相似度的矩阵来
	- 因为这样构建出来的矩阵，根据DPP推导出来的用户点击哪些的概率（也就是如此构建矩阵的determinant）与rank prediction正相关，与彼此之间的相似度负相关。

- ![image.png | 200](assets/image-20220114105335-5jkesqz.png)
	* $q_i$是item i的quality score，由精排提供
	* $D_{ij}$是item i~j之间的距离，可以由粗排提供的embedding计算出来

* $\alpha$, $\sigma$是超参
	* **$\alpha$越大 (i.e., >1)，$L_{ij}$越大，从DPP的角度来看，作为输入的一刷N个item越相似**。
	  * 怎么理解？DPP的目标是为了让结果集中的item差异越大越好。点积时差异很少的，由于乘上一个很大的由于$\alpha$变大，也变得差异很大了，实际上使得最终结果中的多样性就没必要真的那么大了，反正$\alpha$会放大差异性。
	  * Thus, a large α is a good fit for user behavior in the setting where a relatively small subset of the videos in a feed are interacted with
	  * 根据作者经验，**larger $\alpha$, provides better fit for real user data**
	* 注意，这里的$\alpha$, $\sigma$是全局的来平衡相关性与多样性。而实际上需要个性化的平衡因子，因为不同用户对相关性与多样性的要求是不同的。

- 尽管larger $\alpha$(i.e., >1)的拟合效果更好，但是却有可能导致L不再半正定。
	* The PSD condition ensures that the determinants of all submatrices of L are non-negative. This is important because the probability of a set Y is proportional to det(L Y ), and negative “probabilities” do not make sense
	* 解决方法是，先因子分解，去除负的eigen value，再叠加回来。we compute L’s eigendecomposition and replace any negative eigenvalues with zeros


调节超参，比如Grid Search，获得好的$\alpha$和$\sigma$，

* Search出的超参$\alpha$和$\sigma$, 能够使![image.png | 200](assets/image-20220114111839-csptcgq.png)越大越好，j是re-rank后的新index


## 构建Deep Gram矩阵

- 根据后验数据，把DPP所需要的L训练出来
	- we have M training examples, each consisting of: 
	- 1) a set of N items, and 
	- 2) the subset Y of these items that a user interacted with.

- 利用binary cross entropy来训练
	- ![[Pasted image 20220710184626.png |400]]
	- 第二行的公式是因为
		- ![[Pasted image 20220710184518.png |200]]
		- ![[Pasted image 20220710184733.png |200]]

- 把L按照如下形式训练出来
	- ![[Pasted image 20220710185053.png |300]]
	- $q_i,q_j$是精排打分
	- $\phi_i,\phi_j$代表粗排得到的各item embedding
	- `f,g`是要学习的映射函数

## 如何利用DPP重排？

- 现在问题变成了，在**已经知道如何构建L**的前提下，如何在L上DPP来重排？
	- 可以是heuristic方式来构建
	- 也可以根据训练出来的方式来构建

![image.png | 300](assets/image-20220114121002-0eq3cej.png)

* 打给dpp layer的是各item的quality score(精排打分)和item embedding（粗排提供）

* 利用quality score, item embedding构建kernal matrix L，有简单的方法和复杂的方法。构建过程中需要用到一些超参或优化权重，通过搜参或训练的方式得到

  * 我们不是根据DPP采样，而是寻找dpp distribution中的mode，也就是到![image.png | 100](assets/image-20220114114258-zl5rycc.png)
  * 记得Y是user interacted items，we are asking for the size-k set Y that has the highest probability that the user interacts with every one of those k items.
* We then fix some window size k ≪ N, and we ask the DPP for a high-probability set of k videos. 

  * 划分长度=k的窗口，是因为我们没必要要求整个N（输入的item set长度，也是输出的item set长度）都是diverse的
  * k小窗口内部diverse即可，跨窗口可以有相似的item存在
  * N consists of several hundred videos, we use sub-windows where k is a dozen or so videos
* We put these videos at the top of the feed, then again ask the DPP for a high-probability set of k videos from the remaining N − k unused videos.
* These videos become the next k in the feed. We repeat this process until we have ordered the entire feed of N videos.

==真不要脸，最精华的部分，如何实现GreedyApproxMax让它一笔带过，直接给引文==