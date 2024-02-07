---
alias: 
发布时间: 2022-06-14
出品方: stasi
价值: ⭐⭐⭐⭐⭐
环节: 召回
---
关键词:: 
URL::

---

# 作者按
最近忙着写书，知乎和微信公众号拉下了不少。

这篇文章本来是准备放在召回一章中的。但是发现写得太细了，特别是代码解析部分，都写进书里，难免有“贴代码、凑字数、占篇幅”之嫌，所以干脆放出来，以飨读者，也让大家提前感受一下本书的实战化风格。

# 前言
负采样对于召回的重要性，已经在我的《[负样本为王](https://zhuanlan.zhihu.com/p/165064102)》和《[万变不离其宗](https://zhuanlan.zhihu.com/p/345378441)》两篇文章中强调过了。但是只采样了有限几个负样本，如何模拟、逼近“召回”原始的“超大规模多分类”问题，即Approximate Softmax，我之前的文章没有详细说明。

其实这一块还挺乱的，不同的公司有不同的作法，表述上也有差异。比如：
- Airbnb embedding召回，用的是nce loss。你这么说，也没毛病，但是更准确的说法是，它用了negative sampling (NEG) loss。那NCE与NEG到底是什么关系？Negative Sampling? 哪个召回不是negative sampling?
- Youtube里的召回，和Facebook的Que2Search里的召回，都使用了Sampled Softmax。但是youtube里的sampled softmax经过了修正，而Que2Search里没有修正，不论好坏，哪个更有道理？

本文参考的Google的《[candidate sampling](https://www.tensorflow.org/extras/candidate_sampling.pdf)》一文，对NCE、NEG、Sampled Softmax等概念进行了梳理，并解析了TensorFlow对这两种loss的官方实现。

Approximate Softmax所解决的就是Softmax中分母的计算量太大的问题。

$$
\begin{aligned}
L_{sofmax} &=-\log \frac{exp(F(x,y))}{\sum_{y^\prime \in I} exp(F(x,y^{\prime}))} \\
&=-F(x,y)+\log(\sum_{y^\prime \in I} exp(F(x,y^{\prime})))
\end{aligned}
$$
- **x,y是正例**，比如x是user，y是该用户点击过的item
- $y^{\prime}$是x的负例，理论上应该取自整个y的集合（公式中的I）。比如，在u2i召回中，应该取自整个物料集。
- F(x,y)代表x,y之间的匹配度，是我们的模型要建模的目标

问题就在于分母$\sum_{y^\prime \in I} exp(F(x,y^{\prime}))$，理论上需要user和库中所有item都计算一遍，计算量大到不实现，需要近似。怎么近似，又有如下的NCE和Sampled Softmax两种方法。

# Noise contrastive estimation (NCE)

NCE的思想是将extreme large softmax转化为若干个二分类问题。

以u2i举例，描述一下问题：
- 给定一个用户$x_i$
- 他点击过的物料是正例，来自一个集合$T_i$
- 再给$x_i$按照某个概率$Q(y|x)$采样一部分物料当负例，组成负例集合$S_i$。这些负例也就是NCE中的“N”所说的噪声noise
- 那么一个用户$x_i$的所有候选集合，不再像理论softmax那样是整个物料库，而是一个有限集合$C_i=T_i \bigcup S_i$ 

NCE的二分类问题是，对于每个候选物料$\forall y\in C_i$
- y属于$T_i$，算一个正样本，label=1
- y属于$S_i$，算一个负样本，label=0

那么y属于$T_i$的odds有多大？这里的odds可以理解成logit，就是未归一化的概率。应该等于y属于x的正例的概率P(y|x)，与y属于x的负例（即，来自噪声）的概率Q(y|x)，二个概率之差。

即，y对应label=1的logit，G(y|x)=$\log\frac{P(y|x)}{Q(y|x)}=\log P(y|x)-\log Q(y|x)$。

如果使以上公式更通用一些，不再用P(y|x)这样一个表示概率的小数，而是用F(y|x)表示我们模型建模的目标，比如双塔中，F(x,y)就是最后user embedding与item embedding的点积。那么给定一个样本(x,y)，它属于label=1（即$y\in T_i$）的logit，

$G(x,y)=F(x,y) - \log Q(y|x)$

也就是要对模型的输出F(x,y)进行修正，修正量与负采样到相同y的概率Q(y|x)有关。

至于第i个样本上loss，就是$C_i$中每个正负样本上的binary cross-entropy loss之和
$$
\begin{aligned}
L_{NCE} &=-[\sum_{y_i \in T_i}\log\sigma(G(x_i,y_i)) + \sum_{y_i^{\prime}\in S_i}\log(1-\sigma(G(x_i,y_i^{\prime})))] \\
&=\sum_{y_i \in T_i}\log(1+exp(-G(x_i,y_i)))+\sum_{y_i^{\prime} \in S_i}\log(1+exp(G(x_i,y_i^{\prime})))
\end{aligned}
$$
其中$\sigma$是sigmoid函数。而$G(x,y)=F(x,y) - \log Q(y|x)$，是修正后的x,y匹配度。

## Negative Sampling (NEG)
NEG是NCE的又一次简化。如前所述，NCE就是将多分类转化为一系列的二分类问题，二分类binary cross-entropy loss中所使用的公式是修正后的x,y匹配度，$G_{NCE}(x,y)=F(x,y) - \log Q(y|x)$。

而NEG决定进一步简化，就不再修正了，直接拿$G_{NEG}(x,y)=F(x,y)$，代入BCE公式计算loss。

优点：为了修正，计算、存储Q(y|x)还是比较麻烦的，比如要针对全库的item进行离线统计。NEG决定不再修正了，以上麻烦也就省略了，实现起来更简单。

缺点：NCE是有着很强的理论保证的，如果负采样足够多，那么nce loss的梯度与原始超大规模softmax的梯度趋于一致。但是NEG由于忽略了修正，因此没有“趋近原始softmax”的理论保证。但是由于我们大多数时候不关心F(x,y)，只是关心是否学习到高质量的user embedding & item embedding，因此理论上的瑕疵可以忍受，NEG在召回中应用得还是非常广泛的。

# Sampled Softmax

还是用U2I场景来描述问题，给定一个用户$x_i$, 他点击的物料是$t_i$，再给他按照Q(y|x)采样一批负样本$S_i$。原始softmax问题是，在整个物料库中哪个item是$x_i$点击的，现在问题演变成在$x_i$的候选集$C_i=\{t_i\} \bigcup S_i$中，正确挑选出$t_i$的概率是多少，即建模$P(y=t_i | x_i,C_i)$。

假设我们聚焦于第i个样本，以下公式中都省略下标`i`。那么根据条件概率公式展开，$P(y| x,C)=\frac{P(y,C | x)}{P(C | x)}$

再对分子根据bayes公式展开，$P(y,C | x)=P(C | y,x)\times P(y | x)$。公式中的$P(y|x)$就是模型建模的目标，就是归一化后的F(x,y)。

现在聚焦于$P(C|y,x)$，它代表在用户x和某一个物料y已经给定的情况下，构成整个候选集C的概率，它就等于，C中每个物料被采样到的概率，与I-C（I代表整个物料库）中每个物料没被采样到的概率，它们的乘积，即$P(C|y,x)=\prod_{y\prime \in{C-y}}Q(y\prime |x) \times \prod_{y\prime \in {I-C}}(1-Q(y\prime |x))$

把以上公式结合起来，
$$
\begin{aligned}
P(y| x,C)&=\frac{P(y,C | x)}{P(C | x)} \\
&=\frac{P(C | y,x)\times P(y | x)}{P(C | x)} \\
&=\frac{P(y | x) \times \prod_{y\prime \in{C-y}}Q(y\prime |x) \times \prod_{y\prime \in {I-C}}(1-Q(y\prime |x))}{P(C | x)} \\
&=\frac{P(y | x)}{Q(y|x)} \times \frac{ \prod_{y\prime \in C}Q(y\prime |x) \times \prod_{y\prime \in {I-C}}(1-Q(y\prime |x))}{P(C | x)} \\
\end{aligned}
$$
其中第二项$\frac{ \prod_{y\prime \in C}Q(y\prime |x) \times \prod_{y\prime \in {I-C}}(1-Q(y\prime |x))}{P(C | x)}$是与当前预测的y无关的，因此可以写成一个只与$x_i$与$C_i$有关的常数$K_i$，
$$
\begin{align}  
P(y| x,C)&=\frac{P(y | x)}{Q(y|x)} \times K(x,C) \\  
\Rightarrow \log P(y| x,C)&=\log P(y | x)-\log Q(y|x) + K^\prime (x,C) 
\end{align}
$$

再把$\log P(y | x)$ general成F(x,y)。与候选物料y无关的常数项K不影响softmax的结果，忽略掉。最后得到代入softmax的x,y匹配度G(x,y)要写成
$$
G(x,y)=F(x,y)-\log Q(y|x)
$$
与NCE中一样的修正公式，也就是说我们的模型得到F(x,y)（比如user embedding与item embedding的点积）之后，再根据负采样到y的概率Q(y|x)进行修正，修正后的数值才喂入softmax计算loss
$$
\begin{aligned}

L_{SampledSoftmax} &=-\log \frac{exp(G(x,y))}{\sum_{y^{\prime}\in C_i} exp(G(x,y^\prime))} \\
&=-G(x,y)+\log(\sum_{y^\prime \in C_i} exp(G(x,y^{\prime})))

\end{aligned}
$$
# 选择哪种Loss?
NCE Loss和Sampled Softmax Loss在召回中都有广泛运用
- 从word2vec派生出来的算法，如Airbnb和阿里的EGES召回，都使用的是NCE Loss。准确一点说，是NCE的简化版，NEG Loss。尽管NEG Loss在理论上无法等价原始的超大规模softmax，但是不妨碍学习出高质量的embedding。
- 主流的双塔模型，用Sampled Softmax用得比较多。特别是不再负采样了，就拿batch中的其他用户的正例item充当当前user的负例，即对于Batch 'B'中的第i行样本$(x_i,y_i)$，选择$y_j,\forall j\in(B-i)$来当$x_i$的负样本。因为一个batch中所有y的embedding都已经计算好了，这种Batch Sampled Softmax实现起来更简便。

至于哪种更好，业界没有定论，还是需要自己编码实现后，让离线和在线实验告诉我们答案。接下来讲代码实现的时候，我们会看到，nce_loss和sampled softmax loss中大部分实现是共享的，所以实验时切换loss也非常方便。

# 如何定义负采样概率？
注意，NCE与Softmax殊途同归，对于模型得到的x,y匹配度F(x,y)，都要先根据负采样到y的概率Q(y|x)进行修正，$G(x,y)=F(x,y)-\log Q(y|x)$，修正后的G(x,y)才代入公式计算loss。

但是Q(y|x)应该怎么选？简而言之，**热度越高的item，被选中成为负样本的概率应该越高**。这是因为任何一个推荐系统，都难逃“2-8”定律的影响，即20%的热门item占据了80%的曝光量或点击量。
- 训练时，为了降低loss，模型会使每个user embedding尽可能接近少数热门item embedding
- 预测时，每个user embedding从FAISS检索出来的邻居都是那少数几个热门item embedding，**消弱了个性化**。

因此，我们在负采样时，需要**提升热门item成为负例的概率**。可以从两个角度来理解
- 既然热门item已经“绑架”了正例，我们也需要提高热门item在负例中的比例，以**抵销**热门item对loss的影响
- 如果在负采样采取uniform sampling，因为有海量的候选item，而采样量有限，因此极有可能采样得到的item与user“八杆子打不着”，既所谓的**easy negative**。而如果多采集一些热门item当负例，因为绝大多数用户都喜欢热门item，这样采集到的item-是所谓的**hard negative**，会极大地提升模型的分辨能力。

具体原理请参考我在《[推荐系统传统召回是怎么实现热门item的打压?](https://www.zhihu.com/question/426543628/answer/1631702878)》中的回答。

至于如何制订Q(y)来体现“热度越高，越有可能被选中当负例”这一特性，有不同的实现方式。比如，如果我们取经Word2Vec给center word采样不在其context中的negative word的方法，我们可以定义$Q(y)=\frac{f(y)^b}{\sum_{y\prime \in I}f(y\prime)^b}$
- $f(y)$是第i个item的曝光样本数
- b是一个调节因子
- b=1时，负采样完全按照item的热门程度进行，对热门item的打压最厉害，但是对所有候选item的覆盖度下降，导致训练数据环境与预测数据环境的gap增大，反而损害召回效果
- b=0时，负采样变成uniform sampling，对所有候选item的覆盖度最高，减少了训练数据环境与预测数据环境的gap，但是对热门item的打压完全没有打压，采集到的item-都是easy negative，召回效果会偏热门，个性化较差
- 根据word2vec的经验，b一般取0.75

但是以上方法中的Q(y)需要离线统计，可能存在更新不及时而影响效果的问题。Google在《Sampling-Bias-Corrected Neural Modeling for Large Corpus Item Recommendations》提出一种在数据流上直接估计各item出现频率的方法，实现起来有点复杂，感兴趣的同学可以参考之。

在接下来要介绍的TensorFlow自带的`sampled_softmax_loss`和`nce_loss`中，都是根据item的热度从高到低排名，再进行log-uniform采样。

# TensorFlow源码实现

## sampled_softmax_loss
先看`sampled_softmax_loss`的代码。
``` Python
def sampled_softmax_loss(weights,
						biases,
						labels,
						inputs,
						num_sampled,
						num_classes,
						num_true=1,......):
"""
	weights: 待优化的矩阵，形状[num_classes, dim]。可以理解为所有item embedding矩阵，那时num_classes=所有item的个数
	biases: 待优化变量，[num_classes]。每个item还有自己的bias，与user无关，代表自己本身的受欢迎程度。
	labels: 正例的item ids，形状是[batch_size,num_true]的正数矩阵。每个元素代表一个用户点击过的一个item id，允许一个用户可以点击过至多num_true个item。
	inputs: 输入的[batch_size, dim]矩阵，可以认为是user embedding
	num_sampled：整个batch要采集多少负样本
	num_classes: 在u2i中，可以理解成所有item的个数
	num_true: 一条样本中有几个正例，一般就是1
"""
	# logits: [batch_size, num_true + num_sampled]的float矩阵
	# labels: 与logits相同形状，如果num_true=1的话，每行就是[1,0,0,...,0]的形式
	logits, labels = _compute_sampled_logits(......)
	sampled_losses = nn_ops.softmax_cross_entropy_with_logits_v2(
					labels=labels, 
					logits=logits)

	# sampled_losses is a [batch_size] tensor.
	return sampled_losses
```

## nce_loss
再看`nce_loss`的代码。
``` Python
def nce_loss(weights,
			biases,
			labels,
			inputs,
			num_sampled,
			num_classes,
			num_true=1,......):
""" 各输入的含义与sampled_softmax_loss相同
"""
# logits: [batch_size, num_true + num_sampled]的float矩阵
# labels: 与logits相同形状，如果num_true=1的话，每行就是[1,0,0,...,0]的形式
logits, labels = _compute_sampled_logits(......)

# sampled_losses：形状与logits相同，也是[batch_size, num_true + num_sampled]
# 一行样本包含num_true个正例和num_sampled个负例
# 所以一行样本也有num_true + num_sampled个sigmoid loss
sampled_losses = sigmoid_cross_entropy_with_logits(
			   labels=labels,
			   logits=logits,
			   name="sampled_losses")
			   
# We sum out true and sampled losses.
return _sum_rows(sampled_losses)
```

## compute_sampled_logits
从以上代码可以看到，`nce_loss`与`sampled_softmax_loss`是非常相似的，大部分代码都是相同的，集中在`_compute_sampled_logits`中。`_compute_sampled_logits`把user embedding和正负例的item embedding做完点积，再进行修正。至于修正后的结果怎么用，是计算一系列的sigmod cross-entropy loss还是softmax cross-entropy loss，直接代入就是了。
``` Python
def _compute_sampled_logits(weights,
							biases,
							labels,
							inputs,
							num_sampled,
							num_classes,
							num_true=1,
							......
							subtract_log_q=True,
							remove_accidental_hits=False,......):
"""
输入：
	weights: 待优化的矩阵，形状[num_classes, dim]。可以理解为所有item embedding矩阵，那时num_classes=所有item的个数
	biases: 待优化变量，[num_classes]。每个item还有自己的bias，与user无关，代表自己的受欢迎程度。
	labels: 正例的item ids，形状是[batch_size,num_true]的正数矩阵。每个元素代表一个用户点击过的一个item id。允许一个用户可以点击过多个item。
	inputs: 输入的[batch_size, dim]矩阵，可以认为是user embedding
	num_sampled：整个batch要采集多少负样本
	num_classes: 在u2i中，可以理解成所有item的个数
	num_true: 一条样本中有几个正例，一般就是1
	subtract_log_q：是否要对匹配度，进行修正
	remove_accidental_hits：如果采样到的某个负例，恰好等于正例，是否要补救
输出：
	out_logits: [batch_size, num_true + num_sampled]
	out_labels: 与`out_logits`同形状
"""
	# labels原来是[batch_size, num_true]的int矩阵
	# reshape成[batch_size * num_true]的数组
	labels_flat = array_ops.reshape(labels, [-1])

	# ------------ 负采样
	# 如果没有提供负例，根据log-uniform进行负采样
	# 采样公式：P(class) = (log(class + 2) - log(class + 1)) / log(range_max + 1)
	# 在U2I场景下，class可以理解为item id，排名靠前的item被采样到的概率越大
	# 所以，为了打压高热item，item id编号必须根据item的热度降序编号
	# 越热门的item，排前越靠前，被负采样到的概率越高
	if sampled_values is None:
		sampled_values = candidate_sampling_ops.log_uniform_candidate_sampler(
		true_classes=labels,# 正例的item ids
		num_true=num_true,
		num_sampled=num_sampled,
		unique=True,
		range_max=num_classes,
		seed=seed)
		
	# sampled shape: [num_sampled]，一个batch内的所有正样本，共享一批负样本
	# true_expected_count：[batch_size, num_true]，正例在log-uniform采样分布中的概率，接下来修正logit时用得上
	# sampled_expected_count shape = [num_sampled]，负例在log-uniform采样分布中的概率，接下来修正logit时用得上
	sampled, true_expected_count, sampled_expected_count = (
		array_ops.stop_gradient(s) for s in sampled_values)

	# ------------ Embedding
	# labels_flat is a [batch_size * num_true] tensor
	# sampled is a [num_sampled] int tensor
	# all_ids: [batch_size * num_true + num_sampled]的整数数组，集中了所有正负item ids
	all_ids = array_ops.concat([labels_flat, sampled], 0)	
	# 给batch中出现的所有item，无论正负，进行embedding
	all_w = embedding_ops.embedding_lookup(weights, all_ids, ...)
	
	# true_w: [batch_size * num_true, dim]
	# 从all_w中抽取出对应正例的item embedding
	true_w = array_ops.slice(all_w, [0, 0],
		array_ops.stack([array_ops.shape(labels_flat)[0], -1]))

	# sampled_w: [num_sampled, dim]
	# 从all_w中抽取出对应负例的item embedding
	sampled_w = array_ops.slice(all_w,
		array_ops.stack([array_ops.shape(labels_flat)[0], 0]), [-1, -1])

	# ------------ 计算user与每个负例item的匹配度
	# inputs: 可以理解成user embedding，[batch_size, dim]
	# sampled_w: 负例item的embedding，[num_sampled, dim]
	# sampled_logits: [batch_size, num_sampled]
	sampled_logits = math_ops.matmul(inputs, sampled_w, transpose_b=True)
	
	# ------------ 计算user与每个正例item的匹配度
	# inputs: 可以理解成user embedding，[batch_size, dim]
	# true_w：正例item embedding，[batch_size * num_true, dim]
	# row_wise_dots：是element-wise相乘的结果，[batch_size, num_true, dim]	
	......
	row_wise_dots = math_ops.multiply(
		array_ops.expand_dims(inputs, 1),
		array_ops.reshape(true_w, new_true_w_shape))
	......
	# _sum_rows是把所有dim上的乘积相加，得到dot-product的结果
	# true_logits: [batch_size,num_true]
	true_logits = array_ops.reshape(_sum_rows(dots_as_matrix), [-1, num_true])
	......

	# ------------ 修正结果
	# 如果采样到的负例，恰好也是正例，就要补救
	if remove_accidental_hits:
		......
		# 补救方法是在冲突的位置(sparse_indices)的负例logits(sampled_logits)
		# 加上一个非常大的负数acc_weights（值为-FLOAT_MAX）
		# 这样在计算softmax时，相应位置上的负例对应的exp值=0，就不起作用了
		sampled_logits += gen_sparse_ops.sparse_to_dense(
				sparse_indices,
				sampled_logits_shape,
				acc_weights,
				default_value=0.0,
				validate_indices=False)
	
	if subtract_log_q:
		# 对匹配度做修正，对应上边公式中的
		# G(x,y)=F(x,y)-log Q(y|x)
		# item热度越高，被修正得越多
		true_logits -= math_ops.log(true_expected_count)
		sampled_logits -= math_ops.log(sampled_expected_count)

	# ------------ 返回结果
	# true_logits：[batch_size,num_true]
	# sampled_logits: [batch_size, num_sampled]
	# out_logits：[batch_size, num_true + num_sampled]
	out_logits = array_ops.concat([true_logits, sampled_logits], 1)
	
	# We then divide by num_true to ensure the per-example
	# labels sum to 1.0, i.e. form a proper probability distribution.
	# 如果num_true=n，那么每行样本的label就是[1/n,1/n,...,1/n,0,0,...,0]的形式
	# 对于下游的sigmoid loss或softmax loss，属于soft label
	out_labels = array_ops.concat([
		array_ops.ones_like(true_logits) / num_true,
		array_ops.zeros_like(sampled_logits)], 1)

	return out_logits, out_labels
```