#CS188 
尽管 [[Probabilistic Inference#Inference By Emulation]] 可以正确计算查询结果，但在现实情况下，列出整个联合分布并不现实。假如每个变量至多有 $\displaystyle d$ 个取值，对于 $\displaystyle n$ 个变量，联合分布表中的总行数高达 $\displaystyle O(d^n)$。

## Bayes Nets
**贝叶斯网络**(Bayes Net) 可以定义为：
- 一张 **有向无环图**(Directed Acyclic Graph, DAG)，每个结点都表示一个变量 $\displaystyle X$。
- 每个结点的条件概率分布 $\displaystyle P(X|A_{1}\dots A_{n})$，其中 $\displaystyle A_{i}$ 为 $\displaystyle X$ 的第 i 的父节点，可通过 **条件概率表**(Conditional Probability Table, CPT) 存储。每个 CPT 有 $\displaystyle n+2$ 个列：
	- $\displaystyle X$ 的取值
	- 所有 $\displaystyle A_{i}$ 的取值
	- 条件概率 $\displaystyle P(X|A_{1}\dots A_{n})$

贝叶斯网络的结构，本质上反映了不同结点之间的条件独立性关系，而这种独立性很大程度上消除了分布的复杂性。

例如：
![[Pasted image 20260311142635.png]]

在这张图中，我们假定了变量之间 **可能的** 条件关系，而图中未关联（直接或间接）的结点被认为是相互独立的。

此时，利用条件概率公式，可以还原出联合分布：
$$
P(X_{1}, X_{2},\dots,X_{n})=\prod_{i=1}^{n} P(X_{i}|parents(X_{i}))
$$
注意，贝叶斯网络只是 **一种模型**，是为了捕捉现实世界的机制，同时简化计算而存在的，因此必然存在一些误差。不过，好的建模选择依然可以是对世界的优秀近似，足以用来解决现实问题。

