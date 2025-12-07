#Data8 
## Concepts
***Observational study***：数据科学家仅基于观测到的数据作出结论，而没有干预数据的产生。
***Treatment & Outcome***：研究所关心的影响因素称为 *Treatment*，所对应的影响结果称为 *Outcome*。
***Association***：*Treatment* 和 *Outcome* 之间的任意关联。特别地，如果 *Treatment* 导致了 *Outcome* 的发生，那么就称这个 *Association* 是 *Causal* （有因果关系）的。***Causality*** 是许多数据科学研究的核心。

## Comparation
Observational Study 核心的研究方法是 **Comparation**。即，*比较有得到 Treatment 的 Treatment Group 与未得到 Treatment 的 Control Group 的 Outcome*，如果有所不同，那么就可以作为证据支撑 Treatment 与 Outcome 之间的 Association。

## Confounding
> **In an observational study, if the treatment and control groups differ in ways other than the treatment, it is difficult to make conclusions about causality.**

对照组与实验组之间如果除了 Treatment 以外还有其他存在差异的因素，那么这种因素就被称为 Confounding Factor（混杂因子）。

## Randomization
一个避免混杂因子的好方法是，将研究对象***随机地***划分进实验组和对照组，再进行处理和观测。这样做的好处是，可以从统计学上控制两组在其他因素上保持一致，从而消除混杂的影响。这种实验也被称为 ***Randomized Controlled Trial(RCT)***，即***随机对照试验***。

有时，如果测试的对象是人类，那么若测试对象知道自己处于哪一组中，可能会作为混杂因子影响试验的结果。因此，可以进行 ***Blind Experiment***（***盲法试验***），给对照组 Placebo（安慰剂）处理，以消除认知上的混杂。

当然，随机化也有局限性，比如有时随机化的手段并不人道（损害试验人类的健康等），那么就无法进行 RCT。

