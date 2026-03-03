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

因此，我们需要对算法进行优化，称为 $\displaystyle \alpha- \beta$ 剪枝。