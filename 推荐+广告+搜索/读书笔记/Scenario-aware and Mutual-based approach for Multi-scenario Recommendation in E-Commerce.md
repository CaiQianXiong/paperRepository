---
alias: SAML
发布时间: 2020-12-16
出品方: 阿里
价值: ⭐⭐
---

环节:: 
关键词:: #Reco/多场景推荐 

---

# Scenario-aware and Mutual-based approach for Multi-scenario Recommendation in E-Commerce

像是**STAR与HMoE的结合体**

* 有shared dnn还有domain specific dnn，shared dnn与domain specific dnn每层都要交互，这一点是和STAR学习的
	* 比star改进的点是，专门给shared dnn增加了一个loss来进行supervise

* 某个domain的样本，要过一遍所有domain的dnn，这一点是和HMoE学习的
	  * 但是HMoE只在最后做一遍打分之间的交叉
	  * 这里是在底层（或者是每个隐层）让所有domain的输出做一次交叉

* 还有自己独特的，就是相同特征在不同domain下有自己的embedding，有点费资源
* 文章的缺点：写得不清不楚

## Scenario-aware Feature Representation

### Embedding Module

the embedding module explicitly embeds each feature into both global and scenario-speciﬁc subspace in parallel. And then combine the feature vectors from each subspace respectively to construct two types of embedding vectors

* **如果有N个场景，一共要定义1+N套embedding，有点多哟**。==可能也不是所有特征都要独立定义那么多份==。
* ablation test显示比just increase embedding dimension要强

![image.png | 700](assets/image-20220118185956-tz5cjbp.png)

### Attention Module

定义两套attention![image.png | 300](assets/image-20220118202115-ooahcg4.png), "g" stands for global, "l" stands for local.

但是注意，$V_g$是共享的（因为$V_g$受到训练的机会多？）


## Auxiliary Network & Multi-branch Network

**其实和STAR是一样的，都是每层都必须进行shared & domain-specific的交互**。

* 这里的auxiliary network对应STAR中的shared部分。
* 这里的multi-branch对应STAR中的domain specific部分。

作法上就是auxilary network的每层都作为multi-branch的额外输入。

![image.png | 400](assets/image-20220120150252-be7ufih.png)

![image.png | 300](assets/image-20220120150336-l105j4w.png)

* $V_a$是auxilary network的输出，可以看到最底层用的是global embedding那一套
* $V_m$是multi-branch的部分，也就是domain specific的部分，可以看到拿auxilary network的隐层输出当输入
* 很直觉，domain=t的结构参数，只能由domain=t的样本来backprop，其他的都需要stop gradient
* 和STAR不同的地方，对于auxilary部分（也就是shared部分），还加上了一个loss来辅助训练。

## 多场景的相互关系

整体结构里面的下面子结构，内部具体结构看右面

![image.png | 300](assets/image-20220120151007-3f7zis8.png)![image.png | 200](assets/image-20220120151857-gwdunbi.png)![image.png | 300](assets/image-20220120151935-j90zbf2.png)

但是要注意，这里肯定是单独一个domain的样本，让所有domain dnn打分的输出，否则维度对不上。

输入要分M个domain，每个domain的数据，要让M个domain的结构打分，复杂度是$M^2$，想想就😓

只不过是类似HMoE那样，是在最后的打分才做交互，这里是在**底层或中间隐层（这个好像没交待清楚？？？）** 都要让各domain的输出交叉。