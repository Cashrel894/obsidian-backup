#CS188 
**隐马尔可夫链**(Hidden Markov Models, HMM) 是对经典 [[Markov Models]] 的改造。![[Pasted image 20260313111603.png]]

其中，我们有两种类型的结点：**状态变量**(State Variable) $\displaystyle W_{i}$ 和 **证据变量** (Evidence Variable) $\displaystyle F_{i}$。模型约束如下：
$$
F_{1}\perp W_{0} \mid W_{1}
$$
$$
\forall i=2,\dots,n;\space W_{i}\perp \{ W_{0},\dots,W_{i-2},F_{1}\dots,F_{i-1} \} \mid W_{i-1}
$$
$$
\forall i=2,\dots,n;\space F_{i} \perp \{ W_{0}, \dots, W_{i-1}, F_{1},\dots,F_{i-1} \} \mid W_{i}
$$

其中，**转移模型**$\displaystyle P(W_{i+1}\mid P(W_{i}))$ 和 **感知模型**(Sensor Model) $\displaystyle P(F_{i} \mid W_{i})$ 均满足 *静止性*。

最后，我们还可以定义给定前 $\displaystyle i$ 个时间戳的证据时，对第 $\displaystyle i$ 个状态变量的 **信念分布**(Belief Distribution)。
$$
B(W_{i})=P(W_{i} \mid f_{1},\dots,f_{i})
$$
而在缺少 $\displaystyle i$ 时刻的证据时：
$$
B'(W_{i})=P(W_{i} \mid f_{1},\dots,f_{i-1})
$$
## The Forward Algorithm
通过边缘化：
$$
B'(W_{i+1})=P(W_{i+1} \mid f_{1}, \dots, f_{i})=\sum_{w_{i}}P(W_{i+1}, w_{i} \mid f_{1}, \dots, f_{i})
$$
等价于：
$$
B'(W_{i+1})=P(W_{i+1} \mid f_{1}, \dots, f_{i})=\sum_{w_{i}}P(W_{i+1} \mid w_{i},f_{1}, \dots, f_{i})P(w_{i}\mid f_{1}, \dots, f_{i})
$$
即：（**时间流逝更新**, Time Elapse Update）
$$
B'(W_{i+1})=\sum_{w_{i}}P(W_{i+1} \mid w_{i})B(w_{i})
$$
接着可以计算 $\displaystyle B(W_{i+1})$:
$$
B(W_{i+1})=P(W_{i+1}\mid f_{1}, \dots, f_{i+1})=\frac{P(W_{i+1},f_{i+1} \mid f_{1}, \dots, f_{i})}{P(f_{i+1} \mid f_{1}, \dots, f_{i})}
$$
接着，我们暂时推迟归一化，有：（**观察更新**, Observation Update）
$$
B(W_{i+1})\propto P(f_{i+1} \mid W_{i+1})B'(W_{i+1})
$$
因此总表达式为：
$$
B(W_{i+1})\propto P(f_{i+1} \mid W_{i+1}) \sum_{w_{i}}P(W_{i+1} \mid w_{i})B(w_{i})
$$
最后进行归一化，即可得到 $\displaystyle B(W_{i+1})$。

注意，归一化可以一直推迟到最后一步计算，因为在整个算法过程中 $\displaystyle B(W_{i})$ 之间总是成比例的。