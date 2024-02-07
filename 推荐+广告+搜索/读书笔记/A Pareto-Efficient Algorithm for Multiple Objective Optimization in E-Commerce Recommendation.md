---
alias:
发布时间: 2019-09-20
出品方: 阿里
价值: ⭐⭐⭐⭐⭐
---

环节:: 
关键词:: #Reco/多任务学习, 多目标权重

---
# [A Pareto-Efficient Algorithm for Multiple Objective Optimization in E-Commerce Recommendation](http://yongfeng.me/attach/lin-recsys2019.pdf)

![image.png | 400](assets/image-20220120163828-w6z55zs.png)

multi-objective optimization is expected to be **Pareto efficient or Pareto optimality, where no single objective can be further improved without hurting the others**.  It is worth mentioning that Pareto efficient solutions **are not unique** and the set of **all such solutions is named as the “Pareto Frontier”.**


We build our algorithm upon the **KKT** conditions and propose a novel algorithmic **framework** that generates the **scalarization (i.e., loss前的权重)** weights with **theoretical guarantees**

1. we first propose a **condition** for the scalarization weights that ensures the solution is Pareto efficient.
2. The condition is equivalent to a constrained optimization problem, and we propose an algorithm that solves the problem in two steps
    1. we simplify the problem by relaxing the constraints so that an analytic solution is achieved
    2. we get the feasible solution by conducting a projection procedure


## 问题定义

![image.png | 200](assets/image-20220120165525-1cgiknx.png)![image.png | 200](assets/image-20220120170114-wtq3cd9.png)

![image.png | 500](assets/image-20220120165556-l2275i3.png)

除非给定合适的$w_i$集合，否则无法保证解是pareto efficient。接下来，推导出为了让解是pareto efficient而必须加在$w_i$上的条件。


## 目标权重要满足的条件

由原问题求导得到，通过KKT条件，![image.png | 500](assets/image-20220120170224-hnx1djx.png)

上式转化成![image.png | 500](assets/image-20220120170639-9h7p0uy.png) ^x0grvo
- It has been proven that 
	- either the solution to this optimization problem is 0 so that the KKT conditions are satisfied 
	- or the solutions lead to gradient directions that minimizes all the loss functions

## 整体算法框架

* 和模型和loss无关，只要loss是可导的就行
* 使用SGD求解模型的最优参数$\theta$，和通过二次规划求解和loss权重w，交替进行

![image.png | 500](assets/image-20220120195350-wa3qp8t.png)

![image.png | 500](assets/image-20220120172933-xczax0q.png)

这里所谓的[[A Pareto-Efficient Algorithm for Multiple Objective Optimization in E-Commerce Recommendation#^x0grvo |Problem(1)]]就是"权重要满足条件"的原始形式。

## PECsolver

![image.png | 900](assets/image-20220120200231-9gpya3e.png)，先转化成二次规划问题，就有标准做法

### 第一步：只考虑等式约束

We first relax the problem by **only considering the equality constraints** and solve the relaxed problem with an analytical solution. When all the other constraints are omitted except the equality constraints:

![image.png | 500](assets/image-20220120200851-c7h6h4t.png)

有解析解，

![image.png | 300](assets/image-20220120201112-6rtdevx.png)

* ![image.png | 200](assets/image-20220120201137-q66ngfh.png)

* ![image.png | 200](assets/image-20220120201229-295ftgu.png)![image.png | 300](assets/image-20220120201241-425y0ot.png), m是所有待优化参数的个数

* ![image.png | 100](assets/image-20220120201305-gb8wzg1.png)，全1向量

* ![image.png | 500](assets/image-20220120201328-d5x1x98.png)

### 第二步：找出适合解

上一步得到的$\widetilde {w}$，因为是只考虑了等式约束得到的，所以有可能违反了w>0的约束。所以，还需要再解下面的问题，we introduce a projection procedure that generates a valid solution from the feasible set with all the constraints.

![image.png |400](assets/image-20220120202515-q5q3xpd.png)

This problem is exactly a **non-negative least squares problem**, and can be solved easily with the **active set method**.


## 单解和多解

### Pareto Frontier Generation

we can **set different values to the bounds** of the objectives and perform "Algorithm 1"[^1] with **different bounds** respectively. 


### 只要一个解

When the **priorities** of different objectives are available, we can obtain a proper Pareto-efficient recommendation **by setting a proper bound** for the objectives and conduct a single run of "Algorithm 1"[^1].


When the **priorities are not available**, we can first generate the Pareto Frontier and select a solution that is “**fair**” for the objectives.

* One of the most intuitive metrics is Least Misery. in our case, a “Least Misery” recommendation is to minimize the highest loss function of the objectives. ![image.png | 300](assets/image-20220121100711-b4v2o35.png)
* Another frequently used measure is fairness marginal utility, i.e., to select a solution where the cost of optimizing one objective is almost equal to the benefit of the other objectives. ![image.png | 300](assets/image-20220121100936-953ris7.png)(公式没看懂)

接下来就是一个**遍历**的过程，Given the generated Pareto Frontier, the solution with minimum values of Eqn. 5 or Eqn. 6 is selected as the final recommendation


## 电商案例

![image.png | 500](assets/image-20220121103216-lsga9ju.png)

![image.png | 500](assets/image-20220121103304-bphfx7a.png)

![image.png | 500](assets/image-20220121103351-domgklo.png)


## 源码解析

[github](https://github.com/weberrr/PE-LTR)

``` Python

import numpy as np
from scipy.optimize import minimize
import tensorflow as tf

def pareto_efficient_weights(prev_w, c, G):
    """
    G: [K,m]，G[i,:]是第i个task对所有参数的梯度，m是所有待优化参数的个数
    c: [K,1] 每个目标权重的下限约束
    prev_w: [K,1] 上一轮迭代各loss的权重
    """
    # ------------------ 暂时忽略非负约束
    # 对应公式(7-30)
    GGT = np.matmul(G, np.transpose(G))  # [K, K]
    e = np.ones(np.shape(prev_w))  # [K, 1]

    m_up = np.hstack((GGT, e))  # [K, K+1]
    m_down = np.hstack((np.transpose(e), np.zeros((1, 1))))  # [1, K+1]
    M = np.vstack((m_up, m_down))  # [K+1, K+1]

    z = np.vstack((-np.matmul(GGT, c), 1 - np.sum(c)))  # [K+1, 1]

    MTM = np.matmul(np.transpose(M), M)
    w_hat = np.matmul(np.matmul(np.linalg.inv(MTM), M), z)  # [K+1, 1]
    w_hat = w_hat[:-1]  # [K, 1]
    w_hat = np.reshape(w_hat, (w_hat.shape[0],))  # [K,]

    # ------------------ 重新考虑非负约束时的最优解
    return active_set_method(w_hat, prev_w, c)


def active_set_method(w_hat, prev_w, c):
    # ------------------ 对应公式(7-30)
    A = np.eye(len(c))
    cons = {'type': 'eq', 'fun': lambda x: np.sum(x) - 1}  # 等式约束
    bounds = [[0., None] for _ in range(len(w_hat))]  # 不等式约束，要求所有weight都非负
    result = minimize(lambda x: np.linalg.norm(A.dot(x) - w_hat),
                      x0=prev_w,  # 上次的权重作为本次的初值
                      method='SLSQP',
                      bounds=bounds,
                      constraints=cons)
    # ------------------ 对应公式(7-31)
    return result.x + c
```

``` Python

# ------------------ 定义模型
ph_wa = tf.placeholder(tf.float32)  # A loss的权重的占位符
ph_wb = tf.placeholder(tf.float32)  # B loss的权重的占位符
W = ...  # 模型要优化的所有参数

loss_a = loss_fun_a(...)  # A目标的loss
loss_b = loss_fun_b(...)  # B目标的loss
loss = ph_wa * loss_a + ph_wb * loss_b

a_gradients = tf.gradients(loss_a, W)  # A目标loss对所有权重的梯度
b_gradients = tf.gradients(loss_b, W)  # B目标loss对所有权重的梯度

optimizer = tf.train.AdamOptimizer(...)
train_op = optimizer.minimize(loss)  # 优化参数的op

# ------------------ 开始训练
sess = tf.Session()

w_a, w_b = 0.5, 0.5  # 权重初值
c = ...  # 公式(7-26)各权重的下限
for step in range(0, max_train_steps):
    res = sess.run([a_gradients, b_gradients, train_op],
                   feed_dict={ph_wa: w_a, ph_wa: w_b})

    # 当前的梯度矩阵
    G = np.hstack((res[0][0], res[1][0]))
    G = np.transpose(G)

    # 得到新一轮的各目标的最优权重
    w_a, w_b = pareto_efficient_weights(prev_w=np.asarray(w_a, w_b),
                                        c=c,  # 各目标权重的下限约束
                                        G=G)  # 梯度矩阵
```