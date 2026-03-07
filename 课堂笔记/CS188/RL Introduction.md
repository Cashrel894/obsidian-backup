#CS188 
[[Solving MDP]] 是典型的 *离线规划* (Offline Planning)算法，代理对 **转移函数** 和 **奖励函数** 完全了解，可以在执行任何行动之前就预先计算好最优行动。

然而在很多现实问题中并非如此，通常需要 *在线规划*(Online Planning)。在在线规划中，代理必须尝试**探索**(Exploration) 环境，即通过 “执行行动-获得反馈” 的过程增长对环境的认知。这里的“反馈”是指执行行动 $\displaystyle a$ 后，所得新状态与奖励的组合 $\displaystyle (s',R(s,a,s'))$。

经过一定的探索后，代理就可以通过 **强化学习**(Reinforcement Learning) 的过程来估计最佳策略，最终使用该策略来进行 **开发**(Exploitation) 或最大化奖励。![[Pasted image 20260307140011.png]]

在在线规划的每个时间戳，代理从状态 $\displaystyle s$ 出发，采取了行动 $\displaystyle a$，到达目标状态 $\displaystyle s'$，获得奖励 $\displaystyle r$。于是，元组 $\displaystyle (s,a,s',r)$ 就被称为一个 **样本**。

代理通常会持续采取行动、获得样本，直到到达终止状态，在这整个过程中获取的样本集合就被称为一个 **回合**(Episode)。代理通常会进行很多轮的回合进行探索，直到收集到足够的学习数据。

强化学习分为两大类：**基于模型的强化学习**(Model-based Reinforcement Learning, MBRL) 和 **无模型的强化学习**(Model-free Reinforcement Learning, MFRL)。
- [[Model-Based Learning]] 会首先估计转移和奖励函数，再 [[Solving MDP]]。
- [[Model-Free Learning]] 会直接估计状态的 Q-值。