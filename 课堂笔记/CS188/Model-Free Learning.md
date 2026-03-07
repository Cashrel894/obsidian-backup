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

方程的核心在于每个后继状态的 **加权平均值** + 转移奖励。TD 学习试图在不获取权重的情况下 **直接计算**这个加权平均，使用的方法是 **指数移动平均法**(Exponential Moving Average, EMA)。

我们先从初始化 $\displaystyle \forall s,V^\pi(s)=0$ 开始，在每个时间戳，代理会从状态 $\displaystyle s$ 出发，选择行动 $\displaystyle \pi(s)$，转移到状态 $\displaystyle s'$，获得奖励 $\displaystyle R(s,\pi(s),s')$。通过加和奖励与策略 pi 下 $\displaystyle s'$ 的当前 (折扣) 值，我们可以得到一个样本值：
$$
sample=R(s,a,s')+\gamma V^\pi(s')
$$
这个 $\displaystyle sample$ 值就成为 $\displaystyle V_\pi(s)$ 的新估计，通过 EMA 将它与过往估计相结合：
$$
V^\pi(s)\gets(1-\alpha)V^\pi(s)+\alpha \cdot sample
$$
其中，$\displaystyle \alpha$ 被称为**学习率**(Learning Rate)，用于指定新样本值对状态值的贡献权重。通常，$\displaystyle \alpha$ 会从 $\displaystyle 1$ 开始，接着随着学习过程逐渐下降到 $\displaystyle 0$。

如果我们将第 $\displaystyle k$ 轮迭代的 $\displaystyle V^\pi(s)$ 值展开：
$$
V^\pi_{k}(s)\gets\alpha[(1-\alpha)^{k-1}\cdot sample_{1}+\dots+(1-\alpha)\cdot sample_{k-1}+sample_{k}]
$$
不难注意到：越是陈旧的样本数据，在最终的 $\displaystyle V_{k}(s)$ 中所占的比例呈指数级降低。其合理性在于，旧的样本数据使用了旧的 $\displaystyle V^\pi(s')$ 进行计算，其真实性就越低，因此就越不应该贡献 $\displaystyle V^\pi(s)$！这就充分体现出了 EMA 的巧妙之处。

通过这样一条简单的更新规则，我们可以：
- 在每一步都能学习，并使用状态转移相关的信息不断迭代，而不需要等到整个回合结束。
- 给予更旧的、更不准确的样本指数级降低的权重。
- 相比于 [[#Direct Evaluation]]，收敛到真实状态值的速度要快得多，使用的回合数也少得多。

## Q-Learning
[[#Direct Evaluation]] 和 [[#Temporal Difference Learning]] 的目标都是计算**给定策略下的状态值**。然而，我们需要找到的是代理的最优 **策略**，必须计算的是每个状态的 **Q-值**。

因此，以上两种被动强化学习方法通常需要结合一些 [[Model-Based Learning]]，通过估计的 $\displaystyle T$ 和 $\displaystyle R$ 来更新策略。

而主动强化学习方法 **Q-Learning** 则可以**完全绕过**这一过程，无需了解任何有关转移函数、奖励函数甚至状态值的相关信息，是一种纯粹的无模型强化学习方法。

Q-Learning 利用了 **Q 值迭代**(Q-value Iteration)：
$$
Q_{k+1}(s,a)\gets\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma \max_{a'}Q_{k}(s',a')]
$$
这一算法的核心在于 Q 值迭代与 EMA 的有机结合。

对于每次转移，计算 **Q 样本值**
$$
sample=R(s,a,s')+\gamma \max_{a'}Q(s',a')
$$
使用 EMA 更新 Q 值：
$$
Q(s,a)\gets(1-\alpha)Q(s,a)+\alpha \cdot sample
$$
只要我们花费足够的时间进行探索，并以合适的速率降低学习率 $\displaystyle \alpha$，Q-Learning 就会得到每个 $\displaystyle (s,a)$ 的最优 Q-值，从而直接得到最优策略；而 TD 学习和直接评估只能学习特定策略下的状态值，还需要结合其他方法得到最佳策略。

因此，Q-Learning 属于一种 **off-policy learning**，而 TD 学习和直接评估则相对地属于 **on-policy learning**。

## Approximate Q-Learning
Q-Learning 依然有改进的空间。例如，如果一个问题的状态空间非常庞大，存储**每个 Q-值**可能在时间和空间的角度上都非常低效。

而 **近似 Q-学习**(Approximate Q-Learning) 则为对 Q-学习的一个改进。在该算法中，代理不再学习 **每个**状态，而是将相似的状态归纳为 **情形**(Situation)，以削减所需学习的状态数。

其关键在于每个状态有一个 **基于特征的表示**(Feature-Based Representation)，即将每个状态表示为一个 **特征向量**(Feature Vector)。

使用特征向量，就可以将状态值与 Q-值表示为向量各元素的线性组合：
$$
V(s)=w_{1}\cdot f_{1}(s)+w_{2}\cdot f_{2}+\dots+w_{n}\cdot f_{n}(s)=\vec{w}\cdot \vec{f}(s)
$$
$$
Q(s,a)=w_{1}\cdot f_{1}(s,a)+w_{2}\cdot f_{2}(s,a)+\dots+w_{n}\cdot f_{n}(s,a)=\vec{w}\cdot \vec{f}(s,a)
$$
其中 $\displaystyle \vec{w}$ 表示权重向量。定义**差分**(difference)：
$$
difference=[R(s,a,s')+\gamma \max_{a'}Q(s',a')]-Q(s,a)
$$
近似 Q 学习所做的，就是去迭代更新权重向量 $\displaystyle \vec{w}$。更新过程如下：
$$
w_{i}\gets w_{i}+\alpha \cdot difference \cdot f_{i}(s,a)
$$
像这样得到 $\displaystyle w_{i}$ 之后，就可以轻松计算每个 Q 值，同时由于我们只需要存储特征向量，我们可以大大节约空间成本，提高学习效率。

顺带一提，这与普通的 Q 学习在形式上是相似的，其更新规则可以写成：
$$
Q(s,a)\gets Q(s,a)+\alpha \cdot difference
$$
可以理解为，算法计算了样本值与当前值的差值，再以学习率 $\displaystyle \alpha$ 的比例使当前值向样本值方向移动。