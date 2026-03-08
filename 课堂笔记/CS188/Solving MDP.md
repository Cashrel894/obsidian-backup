#CS188 
要解一个 MDP，本质目标是找到一个最优的**策略**（Policy）$\displaystyle \pi^*: S\to A$，即将每个状态 $\displaystyle s \in S$ 映射到行动 $\displaystyle a \in A$ 的一个函数。

这个策略就定义了一个 *反射代理*，其总是会在状态 $\displaystyle s$ 选择行动 $\displaystyle a=\pi(s)$ 作为下一步行动，而不考虑未来的后果。

而最优策略指的是，如只要该代理遵循这个策略行动，最终就能获得最大期望值。

## The Bellman Equation
为了解决 MDP 问题，需要首先引入两个新的数学量：
- 状态 $\displaystyle s$ 的最优值 $\displaystyle V^*(s)$ ，即从 $\displaystyle s$ 出发，在代理的剩余寿命中每一步都进行最优行动，可能取得的最大值。
- Q-状态 $\displaystyle (s, a)$ 的最优值 $\displaystyle Q^*(s, a)$，即从 $\displaystyle s$ 出发，**采取行动** $\displaystyle a$，之后在代理的剩余寿命中每一步都进行最优行动，可能取得的最大值。

从而可以定义上述两个数学量的转移方程：
$$
V^*(s)=\max_{a}\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^*(s')] 
$$
$$
Q^*(s,a)=\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^*(s')]
$$
换句话说：
$$
V^*(s)=\max_{a} Q^*(s,a)
$$

上述方程被称为 **贝尔曼方程**(Bellman Equation)，是 *动态规划方程* 的一个典例。

有了贝尔曼方程，MDP 问题就彻底转换为了 [[Expectimax]] 模型：Q-状态计算子节点的期望 $\displaystyle Q^*(s,a)$，而实际状态则取所有子 Q-状态的最大值 $\displaystyle V^*(s)$。

此外，贝尔曼方程也可以用作确定最优性的条件：如果某个 $\displaystyle V(s)$ 能够满足贝尔曼方程，那么 $\displaystyle V(s)$ 在该 MDP 问题中就是最优值，即 $\displaystyle \forall s \in S,V(s)=V^*(s)$。
