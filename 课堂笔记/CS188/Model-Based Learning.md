#CS188 
在基于模型的强化学习中，代理会：
- 首先，通过记录其进入 Q-状态 $\displaystyle (s,a)$ 后进入状态 $\displaystyle s'$ 的**次数**，以及获得的奖励 $\displaystyle \hat{R}(s,a,s')$。
- 接着，**标准化**统计的进入次数，即将 $\displaystyle (s,a,s')$ 的次数除以进入 Q-状态 $\displaystyle (s,a)$ 的总次数，得到经验概率，作为对转移函数的估计 $\displaystyle \hat{T}(s,a,s')$。

根据 **大数定律**，随着我们收集的样本越来越多，$\displaystyle \hat{T}$ 将收敛到 $\displaystyle T$，而 $\displaystyle \hat{R}$ 将逐渐完善到 $\displaystyle R$。当学习到足够程度后，我们就可以终止探索，通过 [[Value Iteration]] 或 [[Policy Iteration]] 生成开发策略 $\displaystyle \pi_{exploit}$，解决 MDP 问题并取得最大值。

尽管 MBRL 原理非常简单、容易实现，但维护每个 $\displaystyle (s,a,s')$ 可能会变得非常低效。