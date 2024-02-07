
# 特征交叉
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/特征交互 
sort 价值 desc
```


# Embedding
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/Embedding 
sort 价值 desc
```

An Embedding Learning Framework for Numerical Features in CTR Prediction. 华为 2021 #Reco/Embedding

# 新用户冷启
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/新用户冷启
sort 价值 desc
```

Group based Personalized Search by Integrating Search Behaviour and Friend Network
Dropoutnet: Addressing Cold Start in Recommender Systems
STAR-GCN: Stacked and Reconstructed Graph Convolutional Networks for Recommender Systems
CATN: Cross-Domain Recommendation for Cold-Start Users via Aspect Transfer Network

# 新物料冷启
- 待整理
	- Optimal Online Assignment with Forecasts 
	- SHALE: An Efficient Algorithm for Allocation of Guaranteed Display Advertising
	- Improving Cold-start Item Advertisement For Small Businesses
	- A Meta-Learning Perspective on Cold-Start Recommendations for Items
	- Warm Up Cold-Start Advertisements: Improving CTR Predictions via Learning to Learn ID Embeddings


``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/新物料冷启 
sort 价值 desc
```


# 多场景

``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/多场景推荐  
sort 价值 desc
```

---



# 多目标与多任务
``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/多任务学习 
sort 价值 desc
```

---

- [多目标Loss优化策略源码](https://github.com/QunBB/DeepLearning/tree/main/MultiTaskLearning/loss) #价值/⭐⭐⭐⭐ 
- Conversion rate prediction via post-click behaviour modeling
- [Personalized Approximate Pareto-Efficient Recommendation](http://nlp.csai.tsinghua.edu.cn/~xrb/publications/WWW-21_PAPERec.pdf)
- M2GRL: A Multi-task Multi-view Graph Representation Learning Framework for Web-scale Recommender Systems
- [多目标排序模型在腾讯QQ看点推荐中的应用实践](https://zhuanlan.zhihu.com/p/359275468) 腾讯 #Reco/多任务学习


# 多样性/多兴趣
``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/多样性 
sort 价值 desc
```

# 纠偏
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/Debias  
sort 价值 desc
```

- [Bias and Debias in Recommender System: A Survey and Future Directions](https://arxiv.org/abs/2010.03240)
- [KDD Cup 2020 Debiasing比赛冠军技术方案及在美团的实践](https://tech.meituan.com/2020/08/20/kdd-cup-debiasing-practice.html) #TODO


# 因果推断
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #ML/因果推断 
sort 价值 desc
```


# 用户行为序列建模
``` dataview 
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #Reco/用户行为序列     
sort 价值 desc
```

# 在线学习
``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(关键词,"在线学习")
sort 价值 desc
```

# 效果评估
[召回常用评估指标](https://juejin.cn/post/6844904065638350861#heading-7)

``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" and #ML/效果评估 
sort 价值 desc
```

# 模型分析与调优
[推荐系统实用分析技巧](https://zhuanlan.zhihu.com/p/188228577) 
[如何解决离线和线上auc和线上点击率不一致的问题？](https://www.zhihu.com/question/305823078)
[关于深度学习的机理，优化和网络结构的一些个人观点](https://zhuanlan.zhihu.com/p/22067439) 
[算法工程师如何应对业务方和老板的灵魂拷问？](https://zhuanlan.zhihu.com/p/457106185)
Feature Selection for Facebook Feed Ranking System via a Group-Sparsity-Regularized Training Algorithm #Reco/特征重要性 

# 系统架构
[System Architectures for Personalization and Recommendation](https://netflixtechblog.com/system-architectures-for-personalization-and-recommendation-e081aa94b5d8)
[从零开始构建企业级推荐系统 | 完整的推荐系统架构设计](https://mp.weixin.qq.com/s/lheCejiFFKwy-ZdUE9_ncA) #价值/⭐⭐⭐⭐⭐ 

# 实战经验
Improving Deep Learning For Airbnb Search
``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(关键词,"实战经验")
sort 价值 desc
```