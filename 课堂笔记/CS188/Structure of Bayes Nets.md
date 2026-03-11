#CS188 
贝叶斯网络的变量独立性有以下两大性质：
- 一个结点的 **所有父节点** 都给定时，该结点与其所有 **非后代结点** 独立。
![[Pasted image 20260311144313.png]]
- 一个结点的 **马尔可夫毯** (Markov Blanket) 都给定时，这个变量与其他 **所有变量** 独立。
	- 马尔可夫毯包含所有 **父节点**、**子节点** 和 **子节点的其他父节点**。
![[Pasted image 20260311144319.png]]

对于 [[Bayesian Network Representation#Bayes Nets]] 中提到的联合分布公式，有以下直观证明：
- 由于贝叶斯网络是 *有向无环图*，经过 *拓扑排序* 调整顺序，可以写成以下条件概率的乘积：
$$
P(X_{1}, X_{2}, \dots, X_{n})=\prod_{i=1}^{N} P(X_{i}|Parents(X_{i}), Ancestors(X_{i}))
$$
- 同时，我们知道给定 $\displaystyle Parents(X_{i})$ 的情况下，$\displaystyle X_{i}$ 与 $\displaystyle Ancestors(X_{i})$ 独立，故：
$$
P(X_{1}, X_{2}, \dots, X_{n})=\prod_{i=1}^{N} P(X_{i}|Parents(X_{i}))
$$
