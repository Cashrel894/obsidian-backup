#CS188 
**线性回归**（Linear Regression），也称 **最小二乘法**(Least Squares)，是研究 **回归问题**(Regression Problems) 的有效工具。

回归问题的输出是一个连续的值 $\displaystyle y$，特征可以是连续的，也可以是离散的类型值，记为 $\displaystyle n$ 维向量 $\displaystyle x$。

例如，如果我们使用这种线性模型来预测输出：
$$
h_{w}(x)=w_{0}+w_{1}x^1+\dots+w_{n}x^n
$$
为了训练这个模型，我们使用 $\displaystyle L_{2}$ 损失函数衡量模型的预测表现：
$$
Loss(h_{w})=\frac{1}{2}\sum_{j=1}^{N}L_{2}(y^j,h_{w}(x^j))=\frac{1}{2}\sum_{j=1}^{N} (y^j-h_{w}(x^j))^2=\frac{1}{2}\|y-Xw\|^2
$$
其中：
![[Pasted image 20260320092618.png]]

这个损失函数值最小时，必有梯度为 0：
$$
\nabla_{w} \frac{1}{2}\|y-Xw\|^2=-X^Ty+X^TXw=0
$$
即：
$$
\hat{w}=(X^TX)^{-1}X^Ty
$$
接着即可预测：
$$
h_{\hat{w}}(x)=\hat{w}^Tx
$$
 