#CS188 
要解决 CSP 问题，通常使用**回溯搜索**算法。回溯搜索是对 DFS 算法的优化：
1. 固定对变量赋值的顺序，因为赋值是满足交换律的。
2. 当为一个新变量赋值时，只选择那些不与现有赋值冲突的值。当这样的值不存在时，回溯到上一变量。
![[Pasted image 20260228110229.png]]

除此之外，还有进一步优化的策略。

## Filtering
第一个优化策略，就是提前缩减未赋值变量的值域，过滤掉那些必然会导致回溯的值。

一个朴素的方法是**前向检查**(Forward Checking)，即每当变量 $\displaystyle X_{i}$ 被赋值时，就缩减与 $\displaystyle X_{i}$ 有约束关系的变量的值域，删除那些违反与 $\displaystyle X_{i}$ 约束的值。

这一想法可以被推广为**弧一致性**(Arc Consistency)准则。

对于一张约束图，将每一条无向边视作两条有向边，称作**弧**。弧一致性算法步骤如下：
- 初始时将所有弧存储在队列 $\displaystyle Q$ 中。
- 不断从 $\displaystyle Q$ 中删除弧 $\displaystyle X_{i}\to X_{j}$，对于 $\displaystyle X_{i}$ 的每个合法值 $\displaystyle v$，检查其合法性：$\displaystyle X_{j}$ 是否至少存在一个合法取值 $\displaystyle w$，使得 $\displaystyle X_{i}=v,X_{j}=w$ 不违反任何约束。如果不合法，就将 $\displaystyle v$ 从 $\displaystyle X_{i}$ 的合法取值集合中移除。
- 如果在检查弧 $\displaystyle X_{i}\to X_{j}$ 时， $\displaystyle X_i$ 有至少一个取值被移除，那么就将所有弧 $\displaystyle X_{k}\to X_{i}$ 添加到 $\displaystyle Q$，其中 $\displaystyle X_{k}$ 为任意未赋值变量。如果 $\displaystyle X_{k}\to X_{i}$ 已在队列中，则无需添加。
- 不断重复直到 $\displaystyle Q$ 空，或者某些变量的定义域变成空集从而造成回溯。

弧一致性通常使用 AC-3 算法实现：
![[Pasted image 20260228141533.png]]

AC-3 算法的最坏时间复杂度为 $\displaystyle O(ed^3)$，其中 $\displaystyle e$ 为弧的数量，$\displaystyle d$ 为最大定义域的大小。

尽管弧一致性能大大缩减变量定义域，减少回溯，但算法本身就需要更多的计算量，因此选择算法时需要进行权衡。

此外，弧一致性还可以被推广为 **k -一致性**(k-consistency)，即要求对于 CSP 中的每个 k -子集，对于其中每个 (k-1)-子集的一致赋值都能保证剩下一个变量存在一致赋值。

甚至还可以进一步推广为**强 k-一致性**(Strong k-consistency)，不仅要求 CSP 是 k-一致的，还要求满足 (k-1)、(k-2)、……、1-一致。然而，这样做会大大增加计算成本。