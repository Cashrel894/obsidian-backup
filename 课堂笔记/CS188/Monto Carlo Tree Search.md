#CS188 
对于分支系数较高的游戏，如*围棋*，[[Minimax]] 算法复杂度过高，难以采用。此时，我们可以使用***蒙特卡洛树搜索***(Monto Carlo Tree Search, MCTS) 算法。

MCTS 算法基于以下两大核心思想：
- **展开计算**：从一个状态 $\displaystyle s$ 出发，以某种策略（如随机）多次模拟游戏，统计输赢总数。
- **选择性搜索**：搜索没有层数限制，会持续选择性地搜索到叶子结点。
![[Pasted image 20260304112355.png]]

对于每个行动的模拟次数，可以根据胜利希望有所取舍。例如，如果一个行动模拟了 10 次完全没有胜利结果，就可以考虑终止对该行动的搜索，将模拟次数分配到高胜率行动上：
![[Pasted image 20260304112743.png]]

而若两个行动的胜率相近，但其中一个行动的模拟次数较少，此时该行动的胜率分布具有较高的方差，可以考虑优先进行模拟，提高胜率的置信度：![[Pasted image 20260304112917.png]]

综合以上思想，可以使用 **UCB 算法**，来综合权衡胜率和不确定性。

对于每个结点 $\displaystyle n$：
$$
UCB_{1}(n)=\frac{U(n)}{N(n)}+C\times \sqrt{ \frac{{\log N(PARENT(n))}}{N(n)} }
$$
其中 $\displaystyle N(n)$ 表示结点 $\displaystyle n$ 的总展开数，$\displaystyle U(n)$ 表示 $\displaystyle Player(Parent(n))$ 的胜场数。