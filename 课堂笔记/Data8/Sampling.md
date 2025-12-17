#Data8 
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
