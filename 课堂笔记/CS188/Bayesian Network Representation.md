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