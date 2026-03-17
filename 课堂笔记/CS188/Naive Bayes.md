#CS188 
**朴素贝叶斯法**（Naive Bayes）是一种基于贝叶斯公式的分类算法，用于解决 **分类问题** (Classification Problem)，即将一组数据点归类为两个及以上类别中的一类。

在这个问题中，每个数据点都有与之匹配的**标签**(Label)，以及一系列从现实模型中提取出的 **特征** (Features)。特征的选取通常因问题而异，且会对机器学习的结果产生显著的影响。而选取特征的学问就被称为 **特征工程**(Feature Engineering)，是机器学习的基础。

朴素贝叶斯模型通常通过假定***特征之间相互条件独立***，从而简化推理： ![[Pasted image 20260317202234.png]]

此时，预测本质上就是在进行贝叶斯网络推理：
$$
prediction(f_{1}, \dots, f_{n})=argmax_{y}P(Y=y\mid F_{1}=f_{1},\dots, F_{N}=f_{n})=argmax_{y}P(Y=y,F_{1}=f_{1}, \dots, F_{N}=f_{n})
$$
接着有：
$$
prediction(f_{1},\dots,f_{n})=argmax_{y}P(Y=y)\prod P(F_{i}=f_{i} \mid Y=y)
$$
那么，预测的方法有了，接下来只需要从数据中获取每个 $\displaystyle P(F_{i}=f_{i} \mid Y=y)$ 即可。

## Parameter Estimation
假如我们有一组 **样本点**(Sample Point) 或 **观察**(Observation) $\displaystyle x_{1}, \dots, x_{N}$，并且假设这组数据都是从与未知值 $\displaystyle \theta$ 相关的一个分布取样得到，那么应该如何从样本点中 **学习** $\displaystyle \theta$ 的值呢？

一个常用而基础的方法是 **最大似然估计法**(Maximum Likelihood Estimation, MLE)。 MLE 通常进行以下假设：
- 每个样本都是 **独立同分布** (i.i.d.) 的。
- $\displaystyle \theta$ 的每个取值都具有相同的 *先验概率*。

定义 **似然度** (Likelihood) $\displaystyle \mathcal{L}(\theta)=P_{\theta}(x_{1}, \dots, x_{N})$，表示从 $\displaystyle \theta$ 分布中得到样本数据的概率。

又由于 i.i.d. 假设，有：
$$
\mathcal{L}(\theta)=\prod_{i=1}^{N}P_{\theta}(x_{i}) 
$$
要使得似然度最大，必有梯度为 0，从而有必要条件：
$$
\frac{\partial}{\partial \theta}\mathcal{L}(\theta)=0
$$

## Maximum Likelihood for Naive Bayes
