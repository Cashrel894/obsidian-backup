#CS188 
[[HMMs]] 中，精确推理的计算复杂度随着变量定义域的范围增长而增加，在很多情况下是不可接受的。因此，借用 [[Approximate Inference in Bayes Nets]] 的思路，可以使用随机化算法近似估计概率分布。

而在 [[Hidden Markov Models]] 中，这种算法就是 **粒子滤波**(Particle Filtering) 算法。它会模拟一组 *粒子* 在状态图上的运动，从而近似估计随机变量的分布。

在算法过程中，我们会追踪 $\displaystyle n$ 个粒子的状态，每个粒子的状态为随机变量 $\displaystyle d$ 个可能取值中的一个（通常 $\displaystyle n \ll d$）。接着，如果我们想知道时间戳 $\displaystyle t$ 中随机变量的分布，只需考查这 $\displaystyle n$ 个粒子在该时间戳的经验分布即可。

## Particle Filtering Simulation
要模拟一组粒子的运动，类似于 [[Hidden Markov Models#The Forward Algorithm]]，需要在每个时间戳进行如下两次更新：
- **时间流逝更新**：根据转移模型更新粒子的取值。即，对于每个粒子的当前状态 $\displaystyle t_{i}$，从分布 $\displaystyle P(T_{i+1}\mid t_{i})$ 中取样下一个状态值。
- **观察更新**：根据新观测到的证据统一更新所有粒子。具体地，对于一个处于状态 $\displaystyle t_{i}$，观测值为 $\displaystyle f_{i}$ 的粒子，分配一个权重 $\displaystyle P(f_{i} \mid t_{i})$，接着：
	1. 计算所有状态的总权重
	2. 若总权重为 $\displaystyle 0$，重新初始化所有粒子，从头开始。
	3. 否则，归一化位于每个状态上粒子的总权重，得到新的概率分布，将所有粒子从新分布中重新取样。

