#Data8 
**Bootstrap 法**，是指在原有样本中重新取样，得到多个新的随机样本，从而估计统计量的方法。

当我们没有条件对总体进行多次取样时，可以用 Bootstrap 法利用一次样本得到相对准确的估计。

> Here are the steps of the bootstrap method for generating another random sample that resembles the population:
> - *Treat the original sample as if it were the population.*
> - Draw from the sample, at random **with replacement**, the **same number of times as the original sample size**.

**原理**：根据大数定律，重新取样得到的样本分布仍然与原始样本相似，从而与总体相似，故而可以用于对总体的估计中。
![[../../附件/Pasted image 20251221120554.png]]

接下来，为了检验估计的准确程度，可以检查实际值是否落在所有 Bootstrap 样本的估计值的分布的 2.5%~97.5% 范围，以排除偶然因素影响。

> To summarize what the simulation shows, suppose you are estimating the population median by the following process:
- Draw a large random sample from the population.
- Bootstrap your random sample and get an estimate from the new random sample.
- Repeat the above bootstrap step thousands of times, and get thousands of estimates.
- Pick off the “middle 95%” interval of all the estimates.

这种过程被称为 ***Bootstrap 百分位法(The Bootstrap Percentile Method)***。

这样所得到的估计区间，在 95% 的情况下总是能包含实际值。这里的 95% 就被称为***置信度***，得到的估计区间称为***置信区间***。

使用 Bootstrap 的注意事项：
1. 使用一个较大的随机样本。否则这个样本可能不与总体相似，那么 Bootstrap 便不适用。
2. 尽可能多地重复重取样过程。通常以 10000 次为宜。
3. 百分位法有其局限，比如数据不符合正态分布、原始样本量很小等。

使用置信区间，我们还可以用于 [[Testing Hypotheses]]。如果零假设认为某个统计量为某个特定值，那么我们只需要检查这个特定值是否在 p-value 截止值对应的置信区间中（即，5%的截止值对应 95% 置信区间），即可接受或拒绝零假设。
https://inferentialthinking.com/chapters/13/4/using-confidence-intervals/index.html