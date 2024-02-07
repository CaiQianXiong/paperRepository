---
alias:
发布时间: 2021-01-01
出品方: 华为
价值: ⭐⭐
环节: 精排
---

关键词:: #Reco/特征交互 

---
# Enhancing Explicit and Implicit Feature Interactions via Information Sharing for Parallel Deep CTR Models
* 要解决的问题：
  * 一个是insufficent sharing
    * 针对的是DCN中，explicit feature interaction部分与implicit feature interaction部分只在最后才能融合的问题（late fusion）。原来是parallel structure, insufficient sharing。
    * With late fusion, explicit and implicit feature interaction networks **do not share information in the intermediate hidden layers**, which **weakens** the interactive signals between each other and may easily lead to skewed gradients during the backward propagation
  * 另一个是excessive sharing
    * 喂入cross layer与dnn layer都是相同的embedding拼接，作者认为不妥，对于不同的feature interaction应该喂入不同的feature
    * 对于这个问题，创新性比较低，~~不过是加了LHUC而已~~（还不如lhuc，权重是直接定义的优化变量，而不似lhuc那样还有gate input）

* 解决方案，提出Enhanced DCN
  * bridge module，enhance information sharing between explicit and implicit feature interaction
  * regularization module: learn discriminative feature distributions for different networks by a **field-wise gating** network in a soft selection manner.
  * 在每个hidden layer时，也加lhuc来筛选输入，learn discriminative feature distributions for each hidden layer of the two networks.

* 可能存在的问题
  * 由于cross layer和dnn layer要sharing，就强制要求了两个结构中的每层输出都必须具备相同的形状，对模型结构有点不灵活
  * cross layer有一个非常大的缺点，就是不能构建金字塔那样的结构，每层的输出都和原始输入的维度是相同的，而原始输入的维度就是各个field embedding的拼接，如果都接入是相当大的。计算起来有点慢。

## Explicit vs. Implicit 两大类结构

* stacked: Models with stacked structure first perform explicit feature interaction modeling over the embedding layer and then stack an implicit component (normally DNN) to extract high-level feature interactions successively。连DIN/DIEN也算stacked (transformer上再叠加dnn)
* parallel: FM、DCN之类的

![image.png | 600](assets/image-20211227150237-m2l4dmv.png)

## Bridge Module

late fusion有两大缺点：

* The late fusion strategy fails to capture the correlation between two parallel networks in the intermediate layers, weakening the interactive signals between explicit and implicit feature interaction.
* Moreover, **the lengthy updating progress in each sub-networks may lead to skewed gradients during the backward propagation**

所谓bridge module就是cross layer与dnn layer的每层之后，就马上fusion一下。$f_l=f(x_l,h_l)$, where 𝑓 (·) is a pre-defined interaction function that takes two vector as input and **outputs a vector with the same dimension**. fusion函数可以是

* element-wise add
* **element-wise multiplication**。根据作者的实验，**此种方式效果最好**。
* concatenate+fc, $f_𝑙 = ReLU(w_𝑙^T [x_𝑙 , h_𝑙 ] + b_𝑙 )$
* 拿cross layer output与dnn layer output做一个self attention

## Regulation Module

还不如lhuc，直接将各field embedding的权重定义成待优化变量，生学出来。而不似lhuc，起码还有gate input，也就是权重要随输入而变化。

至于在各hidden output中也引入regularization，我只能说有点强词夺理，“Note that the cross network in DCN is actually a linear transformation of $x_0$. In other words, field information still exists in the fused representation of the bridge module.”。cross layer中的确也还能保持field的划分，那么dnn也能吗？