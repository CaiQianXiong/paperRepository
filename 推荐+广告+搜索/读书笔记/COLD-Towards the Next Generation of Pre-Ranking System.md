---
alias: COLD
发布时间: 2020-08-17
出品方: 阿里妈妈
价值: ⭐⭐⭐
---

环节:: 粗排
关键词:: #Reco/特征重要性 

---

# COLD: Towards the Next Generation of Pre-Ranking System 
[阿里粗排技术体系与最新进展分享](https://zhuanlan.zhihu.com/p/355828527)


## 模型优化

其实所谓的将算力也当成优化变量，说白了，==就是在特征输入和链接后面都加上gate，随着主目标优化。随着主目标优化成，gate值也就有了，我们也就知道该删除哪些特征和链接==，简化结构，提升qps。


### 特征筛选

肯定是一个多步走的策略，

* 先预训练，

  * 用SE计算出各特征的权重

    * $s = \sigma(W[e_1,e_2,...,e_M]+b)$, 一共M个field，s就是M长的向量，每一位表示某个field的权重
    * W和b是待优化的参数
    * 预训练的目标与main task的训练目标保持一致，比如也是预估ctr的log loss
  * Then the new weighted embedding $v_i$ is calculated by field-wise multiplication between the embedding $e_i$ and the importance weight $s_i $. 再接下上层DNN
  * 一次训练完毕后，根据s值统计出比较重要的top K fields

    * **这里就出问题了，和Hammer不同，这里s是随着输入而变化的，没有固定的s让我们排序**
    * **论文里没有详细说明，估计是训练时s的平均值来决定field重要与否**
    * 显然，这种se block的输出还和输入有关的情形，肯定不如hammer
  * 以上步骤重复多次，最终在满足QPS和RT要求情况下，选择GAUC最高的一组特征组合，作为COLD最终使用的特征。
  * 注意：**Se Block就是所谓的“将算力作为优化目标”，仅用于特征筛选阶段，线上模型不包含该结构**
* 正式训练，和线上打分，都基于选择出来的特征组合。

### 结构化剪枝

此外COLD还会进行剪枝，做法是在==每个神经元的输出后面乘上一个gamma，然后在训练的loss上对gamma进行稀疏惩罚，当某一神经元的gamma为0时，此时该神经元的输出为0，对此后的模型结构不再有任何影响，即视为该神经元被剪枝==。其实就是"Hammer压缩算法"[^1]。

在训练时，会选用循环剪枝的方式，每隔t轮训练会对gamma为0的神经元进行mask，这样可以保证整个剪枝过程中模型的稀疏率是单调递减的