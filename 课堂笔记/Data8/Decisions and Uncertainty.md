#Data8 
***零假设(The Null Hypothesis)***：是指实际用于模拟数据时所基于的假设。`null` 这一次实际上代表着，如果该假设成立，那么数据的产生只是出于概率而非其他事物。

***检验统计量(The Testing Statistic)***：为了确定零假设是否正确，所需测试的统计量。

***总变差 (TVD, Total Variation Distance)***：指两组分布（或向量）之间***差的绝对值除以 2***。用于刻画两组分布之间的相似程度。TVD 越小，分布越相似。

验证假设的一般步骤:
1. 确定***零假设***（以及对应的***替代假设***）
2. 确定***检验统计量***
3. 模拟得到*零假设下检验统计量的分布*，绘制直方图
4. 比较*检验统计量*的实际观察值和其零假设下的分布，如果两者***一致***，那么零假设成立，反之则不成立。

第四步中所谓的***一致***，其本质是分析者的*主观判断*。

为了减小主观判断的影响，最好遵循某种**统计学惯例**：即计算 P-Value。

***P 值(P-Value，又称显著性水平)***：可粗略定义为，从观察值开始，往偏离零假设方向，得到的尾部分布概率。 P 值越小，观察值就越偏离零假设，就越能支持替代假设的正确性。
> **Definition:** The p-value of a test is the chance, based on the model in the null hypothesis, that the test statistic will be equal to the observed value in the sample or even further in the direction that supports the alternative.

依照惯例：
- P 值小于 **5%** 时，结果在统计学上被称为是“**显著**”的。
- P 值小于 **1%** 时，结果在统计学上被称为是“**高度显著**”的。

