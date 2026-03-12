#CS188 
## Sampling
贝叶斯网络中，另一种计算概率的方式是进行大量的取样，从而近似估计目标概率值。

### Prior Sampling
**先验抽样**(Prior Sampling) 是一种最基础的贝叶斯网络取样方式，它会从零入度结点开始，根据条件概率关系进行随机赋值，最终的赋值结果即为一个样本。缺点在于，对于发生概率较小的情境，所需的样本量将非常之大。

### Rejection Sampling
在先验抽样的基础上，在生成过程中就提前拒绝与证据不一致的样本，即为 **拒绝抽样**(Rejection Sampling)，从而降低生成样本所需的时间成本。

## Likelihood Weighting
**可能性加权抽样**(Likelihood Weighting) 强制将所有证据变量设为目标值，从而完全避免与证据不符的情况。

然而，每个样本的实际概率为 $\displaystyle P(Z_{1},\dots,Z_{p},E_{1},\dots,E_{m})=\prod_{i=1}^{p}P(Z_{i}|Parents(Z_{i}))$（其中 $\displaystyle E_{i}$ 为证据结点，$\displaystyle Z_{i}$ 为其他结点），全概率分布中缺少了证据变量的贡献。

因此，在处理样本时，需要给样本乘上一个 *权重*$\displaystyle w$，而 $\displaystyle w$ 的值即为缺少的 $\displaystyle \prod_{i=1}^{p}P(E_{i}|Parents(E_{i}))$，可以边生成样本边计算。
![[Pasted image 20260311213034.png]]

上述三种抽样方法，准确率都会随样本量的增大而接近 1，其中可能性加权抽样的计算效率最高。

### Gibbs Sampling
**吉布斯抽样**(Gibbs Sampling) 与上述三种抽样有所不同。

在这个算法中：
- 初始状态下，所有变量设置为 **完全随机** 的值（不考虑任何 CPT）。
- 每次抽取一个变量，并根据其他变量此时的取值，对抽取的变量的值重新抽样，重复多次。

进行上述重复抽样的过程足够多次后，我们的样本将最终收敛到正确的分布。![[Pasted image 20260311213613.png]]
