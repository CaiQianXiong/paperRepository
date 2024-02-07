---
alias: DC-GNN
发布时间: 2022-04-25
出品方: 阿里
价值: ⭐⭐⭐
环节: 召回
---
关键词:: #ML/GNN 
URL:: [zotero](zotero://open-pdf/0_EUNIA4FS/1)

---

# DC-GNN: Decoupled Graph Neural Networks for Improving and Accelerating Large-Scale E-commerce Retrieval

## 概述

- 主要面向的是，GNN数据量太大，难于训练的问题
	- 大型互联网系统，本来节点就多，边就多
	- 常规的GNN训练方法，是通过message passing的方式聚合信息。一个节点的k-阶邻居，就是“瓜蔓”一样，指数级别的增多
	- 所以，一般也不敢加深GNN的层数，因为牵扯到的邻居太多，计算量也大。

- 主题就是De-Couple，而De-couple的目的就是为了高效
	- 所谓的de-couple，是解耦“节点embedding”和“结构信息”
	- node representation learning：在pre-training阶段进行，学到各node embedding
	- deep structure：正式训练和预测时进行，用各种order的邻接矩阵，增强node embedding

- 整体结构是双塔
	- 双塔所使用的user embedding/item embedding，是通过两次GNN学习到的
	- 第1层GNN是在预训练阶段，通过常规的link prediction方式学习出各node的embedding
		- 再加上所谓的contrast learning来加强
		- 但是这是在一个异构网络中，怎么区分不同类型的节点❓❓❓
	- 第2次, GNN是在线训练和预测的时候，
		- 其实也压根不是传统意义上的GNN，没有那种层次递进的结构了
		- 而是将整个层次摊平，用所谓线性的方式来将各阶邻居的信息向target node聚合

## 阶段1：预训练

这段是真的没看懂，作者省略太多的细节。

整体来说，由一个要建模的target出发，要Random Walk三次得到三个子图
- 第一个子图是用于“主任务”学习
- 后两个子图是用于“对比辅助学习”❓何必random walk两次，由主任务学习那个再加上另一个random walk的，不就能够组成对比学习的正样本对了吗？

### 主模型
- 第1，为什么要预训练query节点？
	- query不是用户可以随心所欲的组合的吗？有预训练的必要吗？所以这里的query是什么？只是一个单词？

- 第2，这个link prediction的loss很常规，但是有一堆问题
	- q代表query embedding，$i_p$ 代表positive ad embedding，$i_n$ 代表negative ad embedding![[Pasted image 20220524103329.png |400]]
	- 但是作者没说，query embedding和ad embedding是怎么来的？
	- 还是像普通GNN那样，通过聚合邻居信息来的吗？
	- 这个图是一个异构图，里面有query, user, ad三类节点，多种类型边，不同类型的节点是如何沿着不同类型的边传递的呢❓❓❓
	- 另外，文中压根就没提side information的事，所以相当于只有user id/ad id信息去embedding

- $i_n$的选取上，由k-hop来决定难易
	- query->ad->user->query->ad->...
	- 在以上序列中，离query近的ad算是hard negative，远的算是easy negative

### 对比学习
- ![[Pasted image 20220524105004.png |300]]
	- 由同一个target node在两个子图上学习得到的node embedding是相近的，和另一个target node得到的embedding是相远的。
	- ![[Pasted image 20220524104925.png |300]]
	- $q_1$和$q_2$是由同一个query通过两个随机子图学习得到的node embedding
	- $v_2$ 是另一个不相干query得到的node embedding

### 最终目标
![[Pasted image 20220524105209.png |300]]
论文中说，通过以上方法，将各种user, query, ad的embedding都学习出来。

- 按论文里的说法，这时学习出来的embedding代表node attribute，反正我是看不出来。
	- 是接下来的deep aggregation才将structure学进去
	- 在以上学习中，根本没有用到的user&item profile什么的。看样子，只用到了user&ad id。
	- 既然没有用到profile，那么user&ad embedding还不是通过连续关系学出来的？怎么能说没有structure信息呢？

## 阶段2：Deep Aggregation

反正作者认为pre-training里面没有structure信息，再通过本步的deep aggregation"增强"每个node的embedding。

这里的关键是：
- 不像常规GNN那样，多层渐进式的聚合，邻居的邻居先把信息聚合到邻居，经过一层非线性激活，再由邻居聚和到target上
- <span style="color:orange;font-weight:bold">这里是将不同阶数（hop）的邻居铺开</span> 
	- 不同hop的邻居组成一个邻接矩阵，A代表一跳的邻居关系，$A^2$/$A^3$ 代表两跳、三跳的邻居关系
	- X是由第1步预训练得到的所有node embedding
	- 第k跳的邻居的信息向target聚合，表达成$A^kX$💡💡💡
	- 最后将由不同跳数的邻居聚合成的信息，拼接起来![[Pasted image 20220524112148.png |300]]

- 好处：
	- 聚合方式，不再层次递进，可以并发执行
	- 要计算target的embedding，不需要像传染病一样牵扯甚广，是线性的复杂度
	- GNN中有over-smoothing的问题，就是层数太多后，各个node embedding趋于一样，学不出差别了。
		- 这里$h_v^2$中有一部分是X，相当于residual connection，缓解over smoothing

但是这里出现一个大问题❓❓❓
- 无论哪种target，一个target会有三类邻居：neighbor query/user/ad
- 这里A把三种邻居关系都混杂一起了吗？
- 论文中说了，对给定一个target，要区分query/user/ad三种neighbor subgraph
- 所以这里的![[Pasted image 20220524113719.png |100]]只是来源于一种subgraph? 真正的node embedding是三个![[Pasted image 20220524113719.png |100]]再拼接起来❓❓❓

## 阶段3：双塔
最终将“阶段2”得到的query/user/ad embedding喂进合适的塔。

奇怪的是，怎么双塔的负样本选用的是“impressed not click”，愚蠢到家了。









