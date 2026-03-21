#CS188 
在 [[Linear Regression]] 中，我们的输出是一个实数值。那么，如果我们想预测一个分类值呢？

通过 **逻辑回归**(Logistic Regression)，我们可以将输入特征的线性组合转换为对应的概率：
$$
h_{w}(x)=\frac{1}{1+e^{ -w^Tx }}
$$
逻辑函数 $\displaystyle g(z)=\frac{1}{1+e^{-z}}$ 的图像：
![[Pasted image 20260321202025.png]]

$\displaystyle L_{2}$ 损失函数的定义类似：
$$
Loss(w)=\frac{1}{2}(y-h_{w}(x))^2
$$

通过梯度下降估测未知权重 $\displaystyle w$：
![[Pasted image 20260321205035.png]]