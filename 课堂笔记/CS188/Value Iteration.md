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

数列 $\displaystyle V_{k}(s)$ 收敛性的简单证明：![[Pasted image 20260306110941.png]]

也就是说，对于任意两个值函数 $\displaystyle V(s)$ 和 $\displaystyle V'(s)$，经过一次贝尔曼运算符，它们的差之绝对值一定至少收缩 $\displaystyle \gamma$，因此经过无限次贝尔曼运算后必然收敛，极限值为满足贝尔曼方程的不动点 $\displaystyle V^*(s)$。同时，又由于 $\displaystyle V(s)$ 的取值是有限的，因此实际上通过有限次的贝尔曼运算一定可以得到 $\displaystyle V^*(s)$。这就证明了算法的正确性。

## Policy Extraction
得到了最优状态值 $\displaystyle V^*(s)$ 后，还需要提取出最优策略 $\displaystyle \pi^*$ 供代理使用。

我们只需要在每个状态 $\displaystyle s$ 中，选取可以达到 $\displaystyle V^*(s)$ 的行动 $\displaystyle a$ 即可：
![[Pasted image 20260306111812.png]]

由于性能因素，通常使用 Q-值 $\displaystyle Q^*(s,a)$ 来提取策略，因为相比使用 $\displaystyle V^*(s)$ 可以减少一层求和运算。

## Q-value Iteration
实际上，直接使用 $\displaystyle Q^(s)$ 进行值迭代可以得到类似的效果：![[Pasted image 20260306112439.png]]

