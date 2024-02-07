---
alias: ComiRec
发布时间: 2020-01-01
出品方: 
价值: ⭐
---

环节:: 召回
关键词:: #Reco/多样性 

---
# [Controllable Multi-Interest Framework for Recommendation](https://arxiv.org/abs/2005.09347)

## 获得multi-interest embedding

我承认一个embedding可能有点不够，需要多个user interest embedding。但是**你怎么能够保证学习到的各个user embedding有足够的差异性？？？模型上是无法保证这一点的**。


**基于dynamic routing的那一套没太搞明白**，就看self-attention方式获得multi-embedding的方法。说白了，无非就是**attention做好几遍**。

* 简化版的self-attention

  * ![image.png | 400](assets/image-20220112165841-48338ec.png)

    * H就是user action list的矩阵
    * 得到的结果a就是给user action list中每个item的权重。注意，这里的self-attention就得到一套权重，而非n*n的权重矩阵（user action list中每一位对所有邻居的权重）
  * 接着再做一个加权和，就是user embedding ![image.png | 200](assets/image-20220112170344-nee9ghi.png)
* multi-interest self-attention

  * ![image.png | 400](assets/image-20220112170450-cn7bn61.png)

    * 这里的$W_2$是一个$[d_a,K]$的矩阵，$d_a$是attention所需要映射成的common embedding dimension，K是兴趣个数
  * ![image.png | 200](assets/image-20220112170704-6n7c9q0.png)

## 训练

尽管user embeding有K个，但是item embedding只有一个。那么计算user ~ item相似度时要用哪个user embedding？答案是最大的那一个

![image.png  | 400](assets/image-20220112170903-c531rv7.png)![image.png | 400](assets/image-20220112173332-e4b9hia.png)
