---
alias: STAR
发布时间: 2021-11-02
出品方: 阿里妈妈
价值: ⭐⭐⭐
---

环节:: 精排
关键词:: #Reco/多场景推荐

---
# One Model to Serve All: Star Topology Adaptive Recommender for Multi-Domain CTR Prediction

> STAR给出了一种比较直接但有效的解法，具体细节请见STAR论文。事实上STAT开启的视角远比多场景建模宏大，它把我们对通用建模中一些细微的、先验认为具备特征间/样本间差异性的信息，通过对应的输入特征做分叉，对分叉的每一部分设计独立的网络结构，同时分叉点输入给共享的网络结构。这样就能巧妙地把domain knowledge变成网络结构、在输入侧通过某些特征维度增加自由度的形式引入。例如，多场景建模就是在场景特征上增加了自由度


这篇是multi-domain，而非multi-task，有共通之处，也有区别。所谓domain，这里是指淘宝app中不同的应用场景，比如头部的banner，还有猜你喜欢。问题的关键是，**面对同一个label，不同domain下的数据分布有很大不同**。这篇文章，对于我们还是有借鉴意义的，multi-domain对应multi-nation。当然，如果像我们这种，**既是multi-domain又是multi-task的场景，简单地multi-domain * multi-task肯定是吃不消的**。

~~整体评价并不高~~，有点意思

* 整个架构依赖于特殊的数据流，也就是一个batch内的样本都来自一个domain
* 结构上，是拿**share weight和domain specific weight进行elementwise multiplication**得到最终用于前代+回代的权重。这种方式要求share部分与domain specific部分必须有完全相同的结构，而且看不出与`final logit = share logit + domain-specific logit`的区别。
* 拿一个简单结构专门处理domain indicator，然后直接加到final logit中，离loss近一些。目前看来只能算是常规操作。
* 可能最起作用的，还是所谓paritioned normalization，也就是按domain进行normalization。而如果实现起来，也必须和"one batch, one domain"的数据流密切相关的。


in the proposed STAR model, 

* we let all business domains **share the same embedding layer,** （未必是最优的，现在流行分离）i.e., the same ID features in different domains share the same embedding.
* The embeddings are then pooled and concatenated to obtain 𝐵 fixed-length representations. After that, the 𝐵 extracted representations are processed by the proposed **partitioned normalization (PN) layer** that privatizes normalization statistics for different domains.
* The normalized vectors are then fed as input to the proposed star topology FCN to get the output. The star topology FCN consists of shared centered FCN and multiple domain-specific FCNs.
* 最后的输出由三部分组成，The final model of each domain is obtained by combining 

  * **the shared centered FCN**
  * **domain-specific FCN**.
  * features that depict the domain information is of importance. In the STAR model, the auxiliary network treats the **domain indicator as input and fed with other features depicting the domain** to the auxiliary network. **The output of the auxiliary network is combined with the output of the star topology FCN to get the final prediction**.


## Partitioned Normalization

普通"Batch Normalization"[^1]的假设是，all samples are i.i.d. and use the shared statistics across all training samples. However, in multi-domain CTR prediction, **samples are only assumed to be locally i.i.d. within a specific domain**。

训练的时候，**要求某个mini-batch是完全来自某个domain的**，这时partitioned normalization=![image.png | 200](assets/image-20211126103651-m5al19n.png)

* 𝛾 , 𝛽 are the global scale and bias,
* and $\gamma_p, \beta_p$ are the domainspecific scale and bias parameters.
* they are all learnable parameters

compared with BN, PN **also uses the moments of the current minibatch during training**, but PN introduces **domain-specific scale and bias** 𝛾 𝑝 , 𝛽 𝑝 to capture the domain distinction.

during test, PN also let different domains to **accumulate the domain-specific moving average** of mean 𝐸 𝑝 and variance𝑉𝑎𝑟 𝑝 . ![image.png | 200](assets/image-20211126104311-om0fu2z.png)


## 网络结构

很有意思、很新颖的一种将share的部分与domain-specific部分结合的方式，但是看不出哪里好。这里的方式是说，share的部分与domain-specific的部分必须有完全相同的结构，那么对于某个domain，**用于前代+回代的weight是share部分与domain-specific部分的element-wise相乘**

  
![image.png | 200](assets/image-20211128105125-ba2hj9m.png)  
![image.png | 200](assets/image-20211227163139-wdmq5mv.png)

![image.png | 500](assets/image-20211128105146-7spr4jj.png)

* For each domain, the final weights of a neural network layer are obtained by element-wise multiplying the weights of the shared FCN and the domainspecific FCN.
* The shared parameters are updated through the gradient of all examples,
* while the domain-specific parameters are only updated through examples within this domain. ~~这一点是通过每个batch都只来自一个domain来保证的。~~

一个batch内部可以包含多域的数据，可以通过`tf.boolean_mask`来将batch的数据拆分

```python

is_egy = config.get_label("is_egy")
is_kwaime = tf.squeeze(tf.equal(is_egy, 1), axis=1)
is_others = tf.squeeze(tf.equal(is_egy, 0), axis=1)


def split_by_app(inputs):
    """ 根据市场切 batch.
    Args:
        inputs (tensor): [batch_size, dim]
    """
    def split(inputs):
        indices = tf.range(tf.shape(inputs)[0], dtype=tf.int32)
        kwaime_inputs = tf.boolean_mask(inputs, is_kwaime, axis=0)
        kwaime_indices = tf.boolean_mask(indices, is_kwaime)
        others_inputs = tf.boolean_mask(inputs, is_others, axis=0)
        others_indices = tf.boolean_mask(indices, is_others)
        return kwaime_inputs, kwaime_indices, others_inputs, others_indices

    if inputs is None:
        return None, None, None, None
    elif isinstance(inputs, list):
        kwaime_inputs_list = []
        kwaime_indices_list = []
        others_inputs_list = []
        others_indices_list = []
        for one_inputs in inputs:
            a, b, c, d = split(one_inputs)
            kwaime_inputs_list.append(a)
            kwaime_indices_list.append(b)
            others_inputs_list.append(c)
            others_indices_list.append(d)
        return kwaime_inputs_list, kwaime_indices_list, others_inputs_list, others_indices_list
    else:
        return split(inputs)


def merge_by_app(kwaime_inputs, kwaime_indices, others_inputs, others_indices):
    inputs_merge = tf.concat([kwaime_inputs, others_inputs], axis=0)
    indices_merge = tf.concat([kwaime_indices, others_indices], axis=0)
    raw_indices = tf.contrib.framework.argsort(indices_merge)
    outputs = tf.gather(inputs_merge, raw_indices, axis=0)
    return outputs
```

但是~~看不出来这种方式，与share和domain-specific各有各的结构，然后domain-logit = share-logit + domain-specific logit有什么区别？同样能够做到所有样本都更新share部分，只有domain-specific样本才更新domain-specific network的目的，而且实现起来还更简单。~~

其实两种方式，实现起来，还是有比较大的区别的。

* 如果采用parallel structure，只是最后两个logit相加，只是说各domain上的loss，既会影响domain specific weight，也会影响shared weight。但是, **shared tower与domain specific tower各自在信息向上传递时是完全独立的**。
* 如果我们想实现一种，每一层的输出都相加一下，再通过activation的交互方式。以上那种两种logit相加，就实现不了。目前这种domain-specific weight与shared weight按位相乘的方式，反而能让交叉更细化、深入一些。


~~以上公式有一个问题，就是如何用TF来实现？？？~~**其实也好实现，就像论文里面那样，定义两套weight然后再按位相乘就行**

这种不同模块的weight要fusion的作法，和华为"Bridge Module"[^2]的作法有点类似，难道是一种趋势？？而且这里两个weight的融合也用element-wise multiplication，与Bridge Module中的最佳实践也相同。


一开始想想，觉得这种作法类似微信的"Personalized Transfer of User Preferences for Cross-domain Recom..."[^3]，~~有点类似meta network~~。后来再想想，不对。

* meta network是指，每个样本经过的dense parameter是个性化的，
* 而STAR显然不是，每个域的样本要经过的dense parameter虽然是domain specific和shared weight的融合结果，但是两个weight谁也不是personalized的

但是未来未必不能向那个方向去迈进。


## Auxiliary Network

说白了，就是拿domain indicator embedding和其他一些强烈表征domain的特征，拼接一起，再经过一个简单网络（The simple architecture makes the domain features directly influence the final prediction），得到一个logit（类似Wide & Deep）。最终的logit是正式logit和这个auxilary logit之和，再过sigmoid。**没有针对这个auxiliary logit单独做auxiliary loss**，可能只有domain indicator里面的信息有限吧。