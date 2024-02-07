
# 召回

``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(环节,"召回")
sort 价值 desc
```

- [百度搜索召回调研](https://mp.weixin.qq.com/s/Mx2q3AuG92MRWCpeqddMqQ) 
- Cross-Batch Negative Sampling for Training Two-Tower Recommenders 
- PinnerSage: Multi-Modal User Embedding Framework for Recommendations at Pinterest
- Truncation-Free Matching System for Display Advertising at Alibaba #TODO 
	- [nearline(近线)召回在阿里妈妈的实践](https://zhuanlan.zhihu.com/p/413279632)
- Mixed Negative Sampling for Learning Two-tower Neural Networks in Recommendations 
- ESAM: Discriminative Domain Adaptation with Non-Displayed Items to Improve Long-Tail Performance #TODO
	- [全局自适应模块：为召回模型装上第三只眼 | sigir论文解读](https://developer.aliyun.com/article/771589?spm=a2c6h.12873639.0.0.d8d96ee3BehisQ)
- Understanding Negative Sampling in Graph Representation Learning #TODO #Reco/样本策略


# 粗排
[如何提升链路目标一致性？爱奇艺短视频推荐之粗排模型优化历程](https://www.infoq.cn/article/qbdu3ffuvalqx63ubusl) #Reco/全链路优化
[阿里广告技术最新突破：全链路联动-面向最终目标的全链路一致性建模](https://zhuanlan.zhihu.com/p/413240790) #Reco/全链路优化

``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(环节,"粗排")
sort 价值 desc
```


# 精排
- FINN: Feedback Interactive Neural Network for Intent Recommendation.
- [Deep Interest Evolution Network for Click-Through Rate Prediction](https://arxiv.org/abs/1809.03672)
	- [[阿里DIEN] 深度兴趣进化网络源码分析 之 Keras版本](https://www.cnblogs.com/rossiXYZ/p/14331924.html)
- [Practice on Pruning CTR Models for Real-world Systems](https://dlp-kdd.github.io/assets/pdf/DLP-KDD_2021_paper_9.pdf) 
- [阿里妈妈搜索广告CTR模型的“瘦身”之路](https://mp.weixin.qq.com/s/fOA_u3TYeSwAeI6C9QW8Yw) #TODO 


``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(环节,"精排")
sort 价值 desc
```

# 重排

[推荐算法架构4：重排](https://xieyangyi.blog.csdn.net/article/details/123095982#comments_20101608)
[5 ways to increase result diversity at web-scale](https://www.youtube.com/watch?v=bri0C28mfl8&t=1354s)

``` dataview
table 发布时间, 出品方, 环节, 关键词, 价值, alias
from "推荐+广告+搜索/读书笔记" 
where contains(环节,"重排")
sort 价值 desc
```