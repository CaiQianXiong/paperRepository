---
alias: ZEUS
发布时间: 2021-11-05
出品方: 阿里
价值: ⭐⭐
---

环节:: 
关键词:: #Reco/多场景推荐 #ML/自监督学习 

---

# [Self-Supervised Learning on Users' Spontaneous Behaviors for Multi-Scenario Ranking in E-commerce](https://guyulongcs.github.io/files/CIKM2021_ZEUS.pdf)
[CIKM 2021 | 淘宝多场景推荐排序模型ZEUS](https://mp.weixin.qq.com/s/_lVLk_UvWMmmWAH6w6mVpA)

电商搜索全域多场景Query推荐，包括首页底纹、搜索发现、频道页搜索发现、大促会场、联盟APP、特价版等多个场景。其中频道主要包括聚划算、百亿补贴、红包等。

其实这种方法，不太适合我们。**最后上线的时候，还是每个场景有自己单独的模型，从资源和维度上都是不利的**（“RTP找到**这个**场景的排序模型进行打分”）。只不过这多个场景的多个模型，是由一套共同的初始值fine-tune来的。


**多场景Query推荐排序模型的挑战**

1. 反馈闭环问题。通常我们将**点击item作为正样本、未点击item作为负样本**，来训练新的模型上线。导致排序系统**展示出来的item通常具有高频的特点，很多长尾的item没有曝光的机会，马太效应严重**，不利于生态的长期健康成长。
    1. 但是这里的SSL是针对用户的SSL，而不是针对item的，看不出哪里对long-tail item有提升了
2. **小场景和新场景训练数据不足**。
    1. 这个问题，倒是通过SSL解决了，pre-training训练出一个**fast adaptation**的初值
3. **多个场景联合学习。能否同时利用多个场景的数据，建模多个场景的共同点、不同点，提升多场景排序模型的效率呢**？


**解决方案类似MAML的思路**，也就是要**学习一个对所有场景(meta learning中的task)都友好的初值，然后再用各场景下的数据，从这个初值出发，学习出适应各自场景的参数**。只是思路相似，但是用的方法和meta-learning毫不沾边。


## Pre-training
还是基于user action list来做的，但是和"Disentangled Self-Supervision in Sequential Recommenders"不同，这里压根没有将user action list一分为二，前一半预测后一半。

用户有两大类搜索，一种是自发搜索，一种是点击推荐给他的搜索。**文中研究的多场景是指“推荐搜索”，所以干脆拿“自发搜索”作为pre-training的task**。


在pre-training阶段，这时的SSL task不是靠user implicit feedback (点或未点)来作为label，而是预测user next spontaneous search query.

![image.png | 500](assets/image-20220118180739-i0u0ruj.png)

* 主要结构就是所谓"Deep Multifaceted Transformers"(DMT)，说白了就是先self-attention，再在上面做target attention。
* 在sequence of queries, and sequence of clicked products上，分别使用DMT
* 正样本是用户spontaneous search query，负样本是随机采的。1个正样本，配N-1个负样本。
  * 我们采样用户主动搜索query时曝光给用户但用户未点击的query作为hard negative examples。例如，用户在主动输入query时，下拉推荐会展示一些query给用户，我们把这些样本作为负样本
* ![image.png | 300](assets/image-20220118181228-cee53h0.png), $x_{t+1}$就是target spontaneous search query，$c_t$就是之前的历史


## Fine-tuning
分两次fine-tune。loss都是以user implicit feedback为label的cross-entropy loss。

* 首先，**同时使用所有Query推荐场景的implicit feeedback数据进行fine-tuning**，来建模多个场景的共性，得到一个task-adaptive的排序模型，**这个排序模型可以直接用到训练数据少的新场景或小场景**。这个Multi-Scenario Fine-tuning的过程，也可以看成是基于多个场景Query推荐的反馈数据做任务导向的预训练。
* 然后，**使用每个Query推荐场景的implicit feeedback数据对task-adaptive的排序模型继续fine-tuning，** 得到每个场景的排序模型（**每个场景的模型，单独训练，单独部署**）。


We empirically find that the performance of ZEUS is the best when the sequential interest models in the pre-training and finetuning stages share the same global embeddings, which contain embedding vectors for high dimensional sparse ids with large vocabulary size (i.e., **query ids and products ids**). Consequently, in the fine-tuning stage, we **fix the global embedding and only fine-tune other parameters (i.e., embedding vectors of other sparse ids, parameters in the Deep Multifaceted Transformer Layer and the Prediction Layer).** The multiple ranking models for different scenarios use the shared global embedding parameters to represent the commonalities of multiple scenarios, and other scenario-specific parameters to capture the characteristics of each scenario.