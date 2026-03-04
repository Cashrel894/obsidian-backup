#CS188 
对于分支系数较高的游戏，如*围棋*，[[Minimax]] 算法复杂度过高，难以采用。此时，我们可以使用***蒙特卡洛树搜索***(Monto Carlo Tree Search, MCTS) 算法。

MCTS 算法基于以下两大核心思想：
- **模拟计算**：从一个状态 $\displaystyle s$ 出发，以某种策略（如随机）多次模拟游戏，统计输赢总数。
- **选择性搜索**：搜索没有层数限制，会持续选择性地搜索到叶子结点。
![[Pasted image 20260304112355.png]]


## UCB
对于每个行动的模拟次数，可以根据胜利希望有所取舍。例如，如果一个行动模拟了 10 次完全没有胜利结果，就可以考虑终止对该行动的搜索，将模拟次数分配到高胜率行动上：
![[Pasted image 20260304112743.png]]

而若两个行动的胜率相近，但其中一个行动的模拟次数较少，此时该行动的胜率分布具有较高的方差，可以考虑优先进行模拟，提高胜率的置信度：![[Pasted image 20260304112917.png]]

综合以上思想，可以使用 **UCB 算法**，来综合权衡胜率和不确定性。

### UCB criterion
对于每个结点 $\displaystyle n$：
$$
UCB_{1}(n)=\frac{U(n)}{N(n)}+C\times \sqrt{ \frac{{\log N(PARENT(n))}}{N(n)} }
$$
其中 $\displaystyle N(n)$ 表示结点 $\displaystyle n$ 的总展开数，$\displaystyle U(n)$ 表示 $\displaystyle Player(Parent(n))$ 的胜场数。

在这个公式中：
- $\displaystyle \frac{U(n)}{N(n)}$ 为**开发**(Exploitation)项，其值为选择该行动的**经验胜率**，用于衡量 $\displaystyle n$ 的*潜力*。潜力越大，投入的模拟数也越多。
- $\displaystyle C\times \sqrt{ \frac{{\log N(PARENT(n))}}{N(n)}}$ 则为**探索**(Exploration) 项，如果 $\displaystyle n$ 的模拟次数相对较少，这一项的分数就会相对较高，鼓励算法探索模拟次数少、可能具有潜在价值的结点。
	- 系数 $\displaystyle C$ 为“探索系数”，是一个可以调整的超参数，用于平衡开发与探索。

### MCTS UCT algorithm
MCTS UCT 算法使用 **UCT** 公式(Upper Condifidence bounds applied to Trees)，自动化权衡模拟次数。在这里我们利用 UCB 标准，在搜索过程中重复以下步骤：
1. **选择与拓展**：从根结点出发，如果 $\displaystyle n$ 的子节点有结点尚未拓展，则优先拓展该结点；否则，根据 UCB 标准，从结点中选取 UCB 值最高的一个继续深入。遵循以上逻辑，一直深入游戏树直到找到一个未拓展结点为止。
2. **模拟**：对刚刚拓展的结点进行**模拟**（如随机走子直到游戏结束），评估输赢结果。
3. **反向传播**：将模拟的结果沿游戏树返回，更新路径上结点的 $\displaystyle U(n)$ 和 $\displaystyle N(n)$。
最终，当以上步骤被进行足够多次后，选择 $\displaystyle N$ 最高的行动作为代理的下一步决策。注意，$\displaystyle N$ 最高意味着这个行动“最有潜力”，也就最值得采纳，而非根据*胜率* $\displaystyle U$。
