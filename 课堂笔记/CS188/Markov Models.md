#CS188 
**马尔可夫模型**(Markov Model) 是与 [[Bayes Nets]] 密切相关的一种结构，可以被看作一种链状的、可以无限延伸的贝叶斯网络。

例如，如果用 $\displaystyle W_{i}$ 表示第 $\displaystyle i$ 天的天气情况，那么对应的马尔可夫模型：![[Pasted image 20260313093557.png]]

马尔可夫模型满足 **无记忆性**（Memoryless），也就是说，$\displaystyle W_{i+1}$ 只与 $\displaystyle W_{i}$ 条件相关。我们只需要知道 $\displaystyle t=0$ 时的 **初始分布**(Initial Distribution) 以及 **转移模型**(Transition Model)，我们就可以建模整个状态转移序列的概率分布。

例如：
$$
P(W_{0},W_{1},W_{2})=P(W_{0})P(W_{1}\mid W_{0})P(W_{2} \mid W_{1},W_{0})
$$
由于无记忆性，上式等价于：
$$
P(W_{0}, W_{1}, W_{2})=P(W_{0})P(W_{1}\mid W_{0})P(W_{2}\mid W_{1})
$$

故马尔可夫链满足链式法则：
$$
P(W_{0}, W_{1}, \dots, W_{n})=P(W_{0})\prod_{i=0}^{n-1} P(W_{i+1} \mid W_{i})
$$
此外，马尔可夫模型隐含性质是 **静止性**（Stationary）。即，对于任意的 $\displaystyle i$，$\displaystyle P(W_{i+1}\mid P(W_{i}))$ 总是相同的。因此，要表示一个马尔可夫模型，只需要两张概率表：$\displaystyle P(W_{0})$ 和 $\displaystyle P(W_{i+1} \mid P(W_{i}))$。

## The Mini-Forward Algorithm
尽管通过上面的公式，我们可以轻松获得前 $\displaystyle n$ 个状态的联合概率分布。但如果我们要获取某个 $\displaystyle t$ 时刻的状态概率，通常需要边缘化所有其他随机变量才能获取，因此我们需要更加高效的算法。

根据边缘化的定义，有：
$$
P(W_{i+1})=\sum_{w_{i}}P(w_{i},W_{i+1})
$$
通过链式法则，有：
$$
P(W_{i+1})=\sum_{w_{i}}P(W_{i+1}\mid w_{i})P(w_{i})
$$
通过动态规划递推即可计算。

## Stationary Distribution
有时我们想知道，当 $\displaystyle t$ 足够大时，状态的概率分布是否会收敛到一固定值？

**静止分布**(Stationary Distribution) 的定义如下：
$$
P(W_{i+1})=P(W_{i})
$$
不难列出静止分布的方程：
$$
P(W_{t+1})=P(W_{t})=\sum_{w_{t}}P(W_{t+1} \mid w_{t})P(w_{t})
$$
再配合以下条件：
$$
\sum_{w_{t}}P(w_{t})=1
$$
通过解线性方程组，即可解出静止分布。