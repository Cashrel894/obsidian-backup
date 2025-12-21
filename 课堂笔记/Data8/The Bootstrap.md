#Data8 
**Bootstrap 法**，是指在原有样本中重新取样，得到多个新的随机样本，从而估计统计量的方法。

当我们没有条件对总体进行多次取样时，可以用 Bootstrap 法利用一次样本得到相对准确的估计。

> Here are the steps of the bootstrap method for generating another random sample that resembles the population:
> - *Treat the original sample as if it were the population.*
> - Draw from the sample, at random **with replacement**, the **same number of times as the original sample size**.

**原理**：根据大数定律，重新取样得到的样本分布仍然与原始样本相似，从而与总体相似，故而可以用于对总体的估计中。
![[../../附件/Pasted image 20251221120554.png]]
