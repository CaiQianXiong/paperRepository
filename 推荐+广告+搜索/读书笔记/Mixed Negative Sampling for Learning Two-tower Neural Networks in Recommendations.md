---
alias: 
发布时间: 2020-08-20
出品方: Google
价值: ⭐⭐⭐⭐
环节: 召回
---
关键词:: #Reco/双塔模型 #Reco/样本策略 
URL:: [zotero](zotero://open-pdf/0_XNS8JKR9/1)

---

普通的batch negative sampling
![[Pasted image 20220621182858.png |500]]
![[Pasted image 20220621183753.png |300]]
![[Pasted image 20220621183819.png |300]]
A commonly-used sampling strategy for two-tower DNN model is *the batch negative sampling*. Specifically, batch negative sampling treats other items in the same training batch as sampled negatives and therefore the **sampling distribution Q follows the unigram distribution based on item frequency**.

---
只有batch negative sampling，容易造成sample selection bias

It uniformly samples B′ items from another data stream. We refer the additional data stream as **index data, which is a set composed of items from the entire corpus**. These items serve as additional negatives for every single batch
![[Pasted image 20220621184025.png |500]]
B′ candidate items uniformly sampled from the index data are **fed into the right tower**（==难道缓存的不是item embedding而是特征，这部分item embedding还要重新计算？？？==） to produce a B′ × K candidate item embedding matrix $V^{\prime}_B$.

但是问题是，==index data里面的item embedding如何更新？==
To apply MNS, we prepare an index data including all of the candidate apps and corresponding content features with values from the **most recent snapshot**. 论文里没有明说，只能说是最新的item embedding。


![[Pasted image 20220621184351.png |400]]
![[Pasted image 20220621184405.png |300]]
the sampling distribution $Q^*$ becomes a **mixture of item frequency based unigram sampling and uniform sampling, characterized by a ratio between the batch size B and B′**.

