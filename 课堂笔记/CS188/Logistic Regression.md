#CS188 
在 [[Linear Regression]] 中，我们的输出是一个实数值。那么，如果我们想预测一个分类值呢？

通过 **逻辑回归**(Logistic Regression)，我们可以将输入特征的线性组合转换为对应的概率：
$$
h_{w}(x)=\frac{1}{1+e^{ -w^Tx }}
$$
逻辑函数 $\displaystyle g(z)=\frac{1}{1+e^{-z}}$ 的图像：
![[Pasted image 20260321202025.png]]
以概率 $\displaystyle 0.5$ 为分界线，即可实现二分类。

$\displaystyle L_{2}$ 损失函数的定义类似：
$$
Loss(w)=\frac{1}{2}(y-h_{w}(x))^2
$$

通过梯度下降估测未知权重 $\displaystyle w$：
![[Pasted image 20260321205035.png]]

## Multi-Class Logistic Regression
有时，我们想将数据分为 $\displaystyle K$ 类，而非仅仅两类。因此，我们需要建构一个模型，分别输出这 $\displaystyle K$ 类的概率估计。

我们使用 **归一化指数函数**(Softmax Function) 代替逻辑函数，对于一个有特征 $\displaystyle x$、标签为 $\displaystyle i$ 的数据点有：
$$
P(y=i \mid f(x); w) = \frac{e^{ w_{i}^T f(x) }}{\sum_{k=1}^{K} e^{ w_{k}^Tf(x) }}
$$
这样所得到的概率之和为 1，为一个合法的概率分布。

假设我们有 $\displaystyle n$ 个有标签数据点 $\displaystyle (x_{i},y_{i})$，那么样本权重的联合概率分布为：
$$
l(w_{1},\dots, w_{K})=\prod_{i=1}^{n} P(y_{i} \mid f(x_{i}); w)
$$
我们需要最大化这个概率，依然通过梯度进行数学运算即可。