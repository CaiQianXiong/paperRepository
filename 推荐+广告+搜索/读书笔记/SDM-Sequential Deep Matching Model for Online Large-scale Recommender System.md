---
alias: SDM
发布时间: 2019-12-28
出品方: 阿里
价值: ⭐⭐⭐
环节: 召回
---
关键词:: #Reco/用户行为序列 
URL:: [zotero](zotero://open-pdf/0_TATJKW4F/1)

---

# SDM: Sequential Deep Matching Model for Online Large-scale Recommender System

## 看点
- 在召回场景中，从用户的长、短期行为序列中，提取兴趣
- 最后有一个gate来fuse长、短期兴趣

![[Pasted image 20220602162957.png |400]]

## 短期兴趣

用户最后一个session里的行为序列，算是短期行为序列。

1. 先在短期序列上经过一次LSTM
2. 再在LSTM的输出序列上，再做一次Self-Attention
3. 在Self-Attention输出序列上，以user profile为query，再做一次attention，得到用户的短期兴趣表达$s_t^u$ 

## 长期兴趣

除了最后一个session之外，过去7天的行为算是长期行为序列。

1. 先把长期行为序列拆解成若干子序列，比如过去点击过的item id sequence，点击item所属shop的shop sequence等
2. 再拿user profile当query，和每个子序列做attention
3. user profile对每个子序列attention的结果，拼接起来，喂入一个DNN，压缩成一个短向量，就是用户的长期兴趣$p^u$

## 长短期兴趣整合

就是将user profile $e_u$ , 短期兴趣向量$s_t^u$，长期兴趣向量$p_u$ ，喂入一个浅层网络再sigmoid，得到$s_t^u$兴趣向量上的权重$G_u^t$
![[Pasted image 20220602175155.png |400]]
最终用户兴趣向量
![[Pasted image 20220602175224.png |400]]

