#CS188 
**极小极大算法**(Minimax) 是一个经典的零和游戏算法。该算法假定对手总是采取最优行动，即对己方最劣的行动。

**状态值** (State's Value) ：从一个状态出发，能到达的最佳结果。
**终止值** (Terminal Value)：终止状态的值。
![[Pasted image 20260303091623.png]]

称一个状态被一个代理所**控制**，当且仅当该状态为该代理的回合。

![[Pasted image 20260303092538.png]]

己方会尝试最大化状态值，同时敌方会最小化状态值。![[Pasted image 20260303093227.png]]

因此，Minimax 算法会对状态树进行**后序遍历**，在己方控制的状态取状态值最大值，在敌方控制的状态取状态值最小值，从而确定当前行动。![[Pasted image 20260303093442.png]]

## Alpha-Beta Pruning
尽管 Minimax 算法简单、最优、合乎直觉，但它的效率很低，复杂度高达 $\displaystyle O(b^m)$，其中 $\displaystyle b$ 为分支系数，$\displaystyle m$ 为终止结点的预估深度。例如，国际象棋中，$\displaystyle b\approx 35$，$\displaystyle m\approx 100$.

因此，我们需要对算法进行优化，称为 $\displaystyle \alpha- \beta$ 剪枝。![[Pasted image 20260303133508.png]]

这一优化可以将复杂度有效降低到平均 $\displaystyle O(b^{m/2})$，允许搜索的深度翻倍了。

## Evaluation Functions
尽管 $\displaystyle \alpha-\beta$ 剪枝优化了时间复杂度，其依然需要将某些子树持续搜索到终止状态，这对于大部分游戏来说是不可接受的。

因此，我们考虑采用**估价函数**(Evaluation Functions)，输入一个状态，输出该状态对应的 Minimax 估值。通常，越“好”的状态估值越高，反之则越低，和启发函数非常类似。

估价函数在**深度限制 Minimax 算法**(Depth-limited Minimax) 中很实用，我们将预设的最大深度处的非终止结点视为终止结点，并将其估值视作终止值，从而有效限制搜索空间。然而，由于“估计”的不准确性，这将导致 Minimax 算法可能并非最优。

最常见的估值函数是对一系列**特征**(Feature) 的线性组合：
$$
Eval(s)=w_1f_1(s)+w_2f_2(s)+...+w_nf_n(s)
$$
特征是从一个游戏状态中可以提取出来的元素，且这些元素可以被赋予数值值。每个特征都有对应的权重 $\displaystyle w_{i}$ (Weight)，通常一个元素对胜率的贡献越高，权重也越高。

当然，估值函数的形式可以是灵活多样的，例如基于神经网络的非线性估值函数在强化学习中就非常常见。一个好的估值函数往往需要大量的微调和迭代。