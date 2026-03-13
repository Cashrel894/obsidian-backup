#CS188 
关于 [[Hidden Markov Models]]，还有一个需要解答的重要问题：对于给定的证据，隐藏的状态序列 *最有可能* 是什么样的？

也就是说，我们想要解出 $\displaystyle argmax_{x_{1:N}}P(X_{1:N} \mid e_{1:N})$。

这一问题可以通过动态规划算法 **维特比算法**(Viterbi Algorithm) 解决。

维特比算法的整体思路是，先正序递推出最可能状态序列的 *概率*，再逆序反推所选择的状态。

我们将隐藏状态的转移看作在下图上的路径选择，其中每条边的边权为 $\displaystyle P(X_{t} \mid X_{t-1})P(E_{t} \mid X_{t})$，而一条路径的概率则正比于所有边的边权之 *积*。
![[Pasted image 20260313120132.png]]

简明证明：
$$
P(X_{1:N},e_{1:N})=P(X_{1})P(e_{1} \mid X_{1})\prod_{t=2}^{N} P(X_{t} \mid X_{t - 1}) P (e_{t} \mid X_{t})
$$
