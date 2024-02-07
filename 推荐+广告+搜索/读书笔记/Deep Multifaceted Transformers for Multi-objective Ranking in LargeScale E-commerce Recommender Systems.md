---
alias:
发布时间: 2020-10-19
出品方: 京东 
价值: ⭐⭐
出处:
---
环节:: 精排
关键词:: #Reco/用户行为序列 

---

# Deep Multifaceted Transformers for Multi-objective Ranking in LargeScale E-commerce Recommender Systems

说白了就是==先self-attention，然后再target-attention==
* The encoder applies self-attention on the the embeddings of the behavior sequence and allows each item in the sequence to attend over all items in the input sequence.
* the decoder uses the target item as query and the output of the encoder as both keys and values, and learns a unique user interest vector for each target item

![image.png | 400](assets/image-20220118173127-t54aski.png)
