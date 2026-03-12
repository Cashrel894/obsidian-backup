#CS188 
在 [[Games]] 中，我们通过 Minimax 和 Expectimax 来确定能够最大化期望效益的最优行动；[[Bayes Nets]] 可以根据现有的 *证据* 来进行概率推断。

**决策网络**(Decision Networks) 是以上两者的结合，可以基于图概率模型来建模不同行动对效益的影响作用。决策网络由以下成分组成：
- **概率结点**(Chance Nodes)：与贝叶斯网络中的结点相同，概率结点的每个结果都有相应的概率，可以通过贝叶斯网络推断出来。（椭圆）
- **行动结点**(Action Nodes)：代理有权控制的结点，表示从一个行动集合中的选择。（矩形）
- **效益结点**(Utility Nodes)：效益结点是决策网络中概率结点与行动结点的子节点，会根据它们父节点的值来输出相应的效益。（菱形）
![[Pasted image 20260312192034.png]]

对于一个给定的决策网络，代理的最终目标是选择能产出 **最大期望效益**(Maximum Expected Utility, MEU) 的行动。一种直接的算法：
- 固定所有的证据变量，进行贝叶斯网络推理，计算*效益结点*的*所有父概率结点*的后验概率。
- 遍历所有的可能行动，基于刚刚计算的 *后验概率* 计算相应的期望效益：
$$
EU(a|e)=\sum_{x_{1},\dots,x_{n}}P(x_{1},\dots,x_{n}|e)U(a,x_{1},\dots,x_{n})
$$
其中 $\displaystyle x_{i}$ 表示第 $\displaystyle i$ 个概率结点的取值。
- 最后，选择 $\displaystyle EU$ 最高的行动，即得到 $\displaystyle MEU$。

## Outcome Trees
也可以以一种 [[Expectimax]] 树的视角来***展开***决策树：![[Pasted image 20260312193507.png]]
这样的树被称为 **结果树** (Outcome Tree)。此时，行动结点等价于 Maximizer，而概率结点则等价于 Expectizer。