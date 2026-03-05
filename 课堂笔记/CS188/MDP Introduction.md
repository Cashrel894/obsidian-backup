#CS188 
**马尔可夫决策过程**(Markov Decision Processes, MDP) 可以由以下元素定义：
- **状态**的集合 $\displaystyle S$，与传统搜索问题类似。
- **行动**的集合 $\displaystyle A$，与传统搜索问题类似。
- 一个**开始状态**。
- 一个或多个**终止状态**。
- **折扣因子** $\displaystyle \gamma$。
- **转移函数** $\displaystyle T(s, a, s')$，函数值表示从状态 $\displaystyle s$，通过行动 $\displaystyle a$，可以转移到状态 $\displaystyle s'$ 的概率。
- **奖励函数** $\displaystyle R(s, a, s')$。通常，MDP 问题会设置各种奖惩机制，例如每一步普通的状态转移会给较小的**生存**奖励，到达终止状态会给很大的目标达成奖励等等。![[Pasted image 20260305204502.png]]

通常在 MDP 问题中，代理的目标是最大化其所有时间戳的奖励的总和。

MDP问题也可以像图搜索问题一样，转化为搜索树，而问题中的不确定性使用 **Q-状态** (Q-states, a.k.a action states) 进行建模，本质上等同于 [[Expectimax]] 中的期望结点。Q-状态可以使用元组 $\displaystyle (s, a)$ 进行表示，表示在状态 $\displaystyle s$ 采取行动 $\displaystyle a$ 。

![[Pasted image 20260305205608.png]]
上图中绿色的结点即为 Q-状态，用于表示已经接收了行动，但还未转移的中间状态，方便算法进行处理。

## Finite Horizons and Discounting
在很多实际问题中，通常会对时间（即总步数）实施一定的限制。

在 MDP 问题中：
- **有限时域**(Finite Horizons)：限定了代理的“寿命”，对总时间戳数量作出限定。
- **折扣因子**(Discount Factor)：对奖励施加一个随时间指数级下降的系数，鼓励代理高效获取奖励。
$$
U([s0,a0,s1,a1,s2,...])=R(s0,a0,s1)+γR(s1,a1,s2)+γ^2R(s2,a2,s3)+...
$$

可以证明，只要满足 $\displaystyle |\gamma|<1$，等号右侧总是收敛的：![[Pasted image 20260305210409.png]]
这样就可以有效防止代理走无限正循环。

多数情况下 $\displaystyle 0<\gamma<1$，因为负的 $\displaystyle \gamma$ 通常没有现实意义。

## Markovianess
***Markovianess***，是指一个随机过程满足“无记忆性”的特征，即未来的状态与过去的状态是条件独立的。也就是说，我们只需要关注**当前的**状态，而无需考虑到达这个状态的过程，因为过去状态无法给我们关于未来的任何信息。![[Pasted image 20260305210944.png]]

实际上，这种无记忆性是由转移函数所赋予的：
$$
  
T(s,a,s′)=P(s′∣s,a)
$$