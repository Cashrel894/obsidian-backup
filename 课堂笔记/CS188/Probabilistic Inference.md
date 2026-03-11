#CS188 
在人工智能领域中，我们经常需要对多种非确定性的事件之间的关系进行建模。

在之前的章节里，我们对世界的建模总是假定所有状态都是确定且已知的。然而在现实世界里，世界的每个状态都有自己的概率，具体可以建模为一种 **联合分布**(Joint Distribution)：![[Pasted image 20260311135243.png]]
联合分布本质上是一张表格，其中包含了所有可能**结果**(Outcome) （即对联合分布中所有变量的赋值组合）的概率。

## Inference By Emulation
**枚举推理法**(Inference By Emulation) 是最简单的一种计算给定 PDF 下某些变量的概率分布的方法。

对于未知的概率分布 $\displaystyle P(Q_{1}\dots Q_{m}|e_{1}\dots e_{n})$：
1. **查询变量**(Query Variables) $\displaystyle Q_{i}$
2. **证据变量**(Evidence Variables) $\displaystyle e_{i}$
3. **隐藏变量**(Hidden Variables)，即不属于以上两种、但出现在总联合概率分布中的变量。

枚举推理法的算法过程如下：
1. 找出所有与证据变量相一致的行。
2. 计算查询变量在这些行上的边际分布（即加和隐藏变量的所有情况）。
3. **正态化**(Normalize) 查询变量分布，使其概率和为 1。