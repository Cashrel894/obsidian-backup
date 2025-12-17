#Data8 
```python
tbl.sample(n) # 返回由tbl中n个随机行组成的Table。默认有放回
tbl.sample(n, with_replacement=False) # 无放回
```

```python
tbl.sample_proportions(sample_size, model_proportions) # 从一个指定概率分布中抽取sample_size个元素，并计算每个元素采样频率，以与model_proportions相同顺序返回。
```

***确定抽样 (Deterministic Sample)***：直接指定所要采样的元素。例如指定 `[3, 18, 100]` 号元素。
另例：`top.where('Title', are.containing('Harry Potter'))`

***概率抽样 (Probability Sample)***：在采样之前，任意元素子集的**被采样的概率可以被计算**。注意：每个元素的采样概率不一定相同。

***等距抽样 (Systematic Sample)***：将总体中的所有元素排成序列，随机选择一个元素，并以此为基准等距选取其他元素。
```python
"""Choose a random start among rows 0 through 9;
then take every 10th row."""

start = np.random.choice(np.arange(10))
top.take(np.arange(start, top.num_rows, 10))
```

***有放回的随机抽样 (Random Sample With Replacement)***：在一次抽样中，相同的个体**可以**被选取多次。`np.random.choice` 的默认行为。

***简单随机抽样 (Simple Random Sample)***：即***无放回的随机抽样***。在一次抽样中，相同的个体**不能**被选取多次。`np.random.choice` 的 `replace=False` 参数行为。

***方便抽样 (Convenience Sample)***：一种**不值得提倡**的取样方式。即在无法计算采样概率的情况下，随意地进行采样。例：在街角随便找人填写问卷。

***总体参数(Parameter)***：与**总体**相关的数值量。如**总体**的最大值、平均值、中位数等。

***统计量(Statistic)***：使用**采样**得到的数据计算出的数值。如**样本**的最大值、平均值、中位数等。