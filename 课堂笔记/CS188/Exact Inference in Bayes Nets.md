#CS188 
## Variable Elimination
为了通过计算 $\displaystyle P(Q_{1},\dots,Q_{k}|e_{1},\dots,e_{k})$ 进行推理，我们需要消除一些 *隐藏变量*。为了消除变量 $\displaystyle X$，我们：
1. 将有关 $\displaystyle X$ 的所有 *因子* 相乘。
2. 通过**加和**消除 $\displaystyle X$。

其中，**因子**(Factor) 可以简单地定义为一个尚未归一化的概率。在变量消除的过程中，每个因子会与其对应的概率成比例，但和不一定为 1，用于表示贝叶斯网络的部分概率结构。伪代码如下：
![[Pasted image 20260311175419.png]]

变量消除的本质是：
![[Pasted image 20260311181910.png]]
等价于：
![[Pasted image 20260311181918.png]]