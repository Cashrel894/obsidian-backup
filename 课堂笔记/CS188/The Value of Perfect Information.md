#CS188 
在现实世界中，代理不一定拥有对世界的完整信息，需要不断收集更多的证据来决定下一步行动。

**完全信息价值**(The Value of Perfect Information, VPI)，用于度量观测到新证据后，代理 MEU 的期望增加幅度。VPI 的意义在于可以与观测新证据的代价进行比较，从而决定是否需要观测新证据。

## General Formula
$\displaystyle MEU$ 公式：
$$
MEU(e)=\max_{a}\sum_{s}P(s\mid e)U(s,a)
$$
假如观测到了证据 $\displaystyle e'$：
$$
MEU(e,e')=\max_{a}\sum_{s}P(s\mid e, e')U(s,a)
$$
然而我们此时并不知道会观测到什么证据，因此新证据应表示为一个 *随机变量*，总的新 $\displaystyle MEU$ 以**期望**的方式计算：
$$
MEU(e,E')=\sum_{e'}P(e'\mid e)MEU(e,e')
$$
根据 $\displaystyle VPI$ 的定义，可得计算公式：
$$
VPI(E'\mid e)=MEU(e,E')-MEU(e)
$$
这里的 $\displaystyle VPI(E' \mid e)$ 就表示 **观测新证据 E' 的价值**。

## Properties of VPI
VPI 有一些重要性质：
- **非负性**(Nonnegativity)：$\displaystyle \forall E',e\space VPI(E'\mid e)\geq 0$。观测更多的证据意味着掌握了更多的信息，作出的决策也必然更优或相等。
- **非加性**(Nonadditivity)：$\displaystyle VPI(E_{j},E_{k} \mid e)\neq VPI(E_{j}\mid e) + VPI(E_{k} \mid e)$。观测一个证据经常会影响另一个证据的观测价值，不能简单相加。
- **次序独立性**(Order Independence)：$\displaystyle VPI(E_{j},E_{k} \mid e)=VPI(E_{j} \mid e) + VPI(E_{k} \mid e, E_{j})=VPI(E_{k}\mid e) + VPI(E_{j} \mid e, E_{k})$。观测证据的顺序并不影响观测这些证据的总价值。
