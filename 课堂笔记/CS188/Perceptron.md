#CS188 
## Linear Classifiers
[[Naive Bayes]] 的核心思想是从特征数据中提取特定的属性，从而根据估测已知特征条件下的标签概率：$\displaystyle P(y \mid f_{1}, f_{2}, \dots, f_{n})$。

这要求我们估测完整的概率分布，然而有时我们只想知道一个数据点属于哪一类，例如**二分类**(Binary Classification)。

为解决这个问题，一个简单的模型是 **线性分类器**(Linear Classifier)。其基本思想是通过对特征的线性组合值，即 **激活值**(Activation) 来进行分类：
$$
activation_{w}(x)=h_{w}(x)=\sum_{i}w_{i}f_{i}(x)=w^Tf(x)=w\cdot f(x)
$$
接着利用激活值进行分类：
![[Pasted image 20260319100523.png]]

几何图像：
![[Pasted image 20260319100558.png]]

故等价于：
![[Pasted image 20260319100610.png]]

 $\displaystyle w^Tf(x)=0$ 的点组成的集合，被称为 **决策边界**(Decision Boundary)，是维度比数据空间恰好少 1 的超平面，将整个空间分为两个部分，即为分成的两类。

## Binary Perceptron
**二元感知器**(Binary Perceptron) 的目的是找到一个分离训练数据的最佳决策边界，即找到最佳的权重向量 $\displaystyle w$，使得训练数据可以被正确分类的概率最高。

感知器算法过程如下：
1. 将权重向量设为 0：$\displaystyle w=0$
2. 对于每个训练样本，记特征为 $\displaystyle f(x)$，真实标签 $\displaystyle y^* \in \{ -1, +1 \}$：
	1. 使用当前权重分类样本，令 $\displaystyle y$ 为 $\displaystyle w$ 给出的标签：![[Pasted image 20260319101514.png]]
	2. 将预测标签 $\displaystyle y$ 与真实标签 $\displaystyle y^*$ 进行比较：
		- 若 $\displaystyle y=y^*$，说明预测准确，无需更新；
		- 否则，更新权重：$\displaystyle w \gets w + y^*f(x)$
	3. 如果遍历所有训练样本后，权重都无需更新，则算法停止；否则，重复步骤 2.

## Updating Weights
让我们深入考查权重更新规则：
$$
w \gets w + y^*f(x)
$$
更新可以分为两种情况：
- 误将正值分类为负值：$\displaystyle w \gets w + f(x)$
- 误将负值分类为正值：$\displaystyle w \gets w - f(x)$

这样的更新规则本质上是在 **补偿** 分类的错误。不妨考查 $\displaystyle w \gets w + f(x)$：
$$
h_{w+f(x)}(x)=(w+f(x))^Tf(x)=w^Tf(x)=f(x)^Tf(x)=h_{w}(x)+f(x)^Tf(x)
$$
在正值误被分类为负值的情况下，这样的更新操作必然会使得样本的激活值增大，从而更加靠近正确的正值。

那么，为什么要加减特征向量，而不是其他的什么东西呢？这是因为，特征向量中绝对值越大的部分对激活值的贡献就越大，