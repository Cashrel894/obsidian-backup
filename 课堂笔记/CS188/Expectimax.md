#CS188 
[[Minimax]] 算法假定对手总是作出最优行动，导致其对局面的估计往往过于悲观，特别是在对手的行动难以预测的情况下。例如，游戏的转移模型本身就具有随机性，或者对手的行动可能是次优的。

因此，我们需要将随机性与概率纳入算法，将 [[Minimax]] 推广到**期望最大化算法**(Expectimax)。

Expectimax 与 Minimax 的最大不同在于，Minimax 侧重考虑对手行动**最坏情形**，Expectimax 则考虑的是对手行动的**平均情形**： ![[Pasted image 20260304102954.png]]

伪代码如下：
![[Pasted image 20260304103158.png]]

## Mixed Layer Types
基础的 Minimax 和 Expectimax 算法要求敌我回合交替进行，然而在很多游戏中并非如此。例如，在吃豆人中，当 Pacman 行动后，可能有多个 Ghost 需要同时选择行动。

此时，假定当前游戏有四个 Ghost，那就可以在 Pacman 的 Maximizer 层后增加四个连续的 Ghost Minimizer。这个时候，所有 Minimizer 就可以自然地开始“合作”，共同最小化状态值。甚至不同的敌对层之间也可以存在差异化，例如有些 Ghost 随机移动，有些 Ghost 则用 Minimizer 采取最优行动。

