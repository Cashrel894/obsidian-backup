#CS188 
[[Value Iteration]] 的收敛通常非常缓慢，在每次迭代中需要计算 $\displaystyle |S||A|$ 个 Q-值，而每个 Q-值需要遍历 $\displaystyle |S|$ 个状态，单次迭代的总复杂度可达 $\displaystyle O(|S|^2|A|)$。此外，策略提取的效率要比值迭代高得多，这提示了策略本身的收敛可能要比值迭代更快。

因此，可以选用另一种通常更快的迭代方式，**策略迭代**(Policy Iteration)：
1. 定义一个初始策略。策略的选取可以是任意的，但策略越接近最优，收敛速度越快。
2. 重复以下过程，直到收敛：
	- 使用 **策略评估**(Policy Evaluation)求解当前策略下，所有状态的期望值。即，对于一个策略 $\displaystyle \pi$，对所有状态 $\displaystyle s$ 计算 $\displaystyle V^\pi(s)$，$\displaystyle V^\pi(s)$ 为从状态 $\displaystyle s$ 出发，遵从策略 $\displaystyle \pi$，可得的期望效益：$\displaystyle V^\pi(s)=\sum_{s'}T(s,\pi(s),s')[R(s,\pi(s),s')+\gamma V^\pi(s')]$。由于没有 $\displaystyle max$ 运算符，相当于解一个 $\displaystyle |S|*|S|$ 的线性方程组，可以方便地进行求解。除此之外，也可以用类似 [[Value Iteration]] 的方式进行迭代直到收敛，但通常比解线性方程组低效。
	- 得到评估结果后，使用 **策略改进**(Policy Improvement) 生成更好的策略：$\displaystyle \pi_{i+1}(s)=argmax_{a}\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^{\pi_{i}}(s')]$ 。如果 $\displaystyle \pi_{i+1}=\pi_{i}$，那么策略收敛，此时 $\displaystyle \pi_{i+1}=\pi_{i}=\pi^*$。

