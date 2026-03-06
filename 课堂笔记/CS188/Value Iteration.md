#CS188 
有了 [[Solving MDP]] 中建立的数学模型，接下来就需要一套算法来计算出最终结果。

为此，我们需要引入**时限值**(Time-Limited Value) $\displaystyle V_{k}(s)$，表示从状态 $\displaystyle s$ 出发，时间限制为 $\displaystyle k$，能到达的最大效益。这等效于在搜索树上进行 $\displaystyle k$ 层 [[Expectimax]] 算法。

**值迭代**(Value Iteration) 是一种**动态规划算法**：每次迭代使用比上次迭代更长的时间限制 $\displaystyle k + 1$，直到收敛，即 $\displaystyle \forall s,V_{k+1}(s)=V_{k}(s)$。具体步骤如下：
1. $\displaystyle \forall s \in S$，初始化 $\displaystyle V_{0}(s)=0$。
2. 重复以下更新，直到收敛为止：
$$
\forall s\in S,V_{k+1}(s)\gets \max_{a}\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V_{k}(s)]
$$
当达到收敛状态时，必满足贝尔曼方程，即 $\displaystyle \forall s \in S, V_{k}(s)=V_{k+1}(s)=V^*(s)$。

为了方便起见，有时会用 $\displaystyle V_{k+1} \gets BV_{k}$ 来表示上述更新规则，其中 $\displaystyle B$ 被称为 *贝尔曼运算符*。

