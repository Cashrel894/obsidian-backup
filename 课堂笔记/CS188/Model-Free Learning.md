#CS188 
**无模型强化学习**分为两大类：**被动强化学习**和**主动强化学习**。
- 被动强化学习(Passive Reinforcement Learning)：代理会遵循给定的策略，学习在该策略下的状态值，与 [[Policy Iteration]] 类似。
- 主动强化学习 (Active Reinforcement Learning)：代理会使用它获得的反馈，主动地迭代更新它的策略，直到经过足够的探索到达最优策略。

## Direct Evaluation
**直接评估法**(Direct Evaluation) 属于被动强化学习，在未知环境中估计某个策略的效益。它会固定代理的策略 $\displaystyle \pi$，让代理使用 $\displaystyle \pi$ 进行多个回合。在代理遵循 $\displaystyle \pi$ 收集样本的过程中，代理会维护从每个状态中获得的期望效益 $\displaystyle V^\pi(s)$。![[Pasted image 20260307144208.png]]

尽管直接评估法会最终收敛到每个状态的状态值，但它收敛得很慢，因为它忽略了状态之间的转移。

## Temporal Difference Learning
**时序差分学习**(Temporal Difference Learning, TD Learning) 也属于被动强化学习。不同于 [[#Direct Evaluation]]，它会从每次回合 *经验* 中学习，而不是只维护每个状态的期望值。

在 [[Policy Iteration]] 中，需要通过解一系列固定策略的贝尔曼方程来评估一个策略。![[Pasted image 20260307151505.png]]

方程的核心在于每个后继状态的 **加权平均值** + 转移奖励。TD 学习试图