#CS188 
有时，像 [[Linear Regression]] 那样将梯度设为 0 再解出权重的方式可能没有效果，因为闭合解不存在。

通常，我们会使用 **基于梯度的方法**(Gradient-based Methods) 来找到最佳权重。

梯度总是会指向使得函数值变化最快的方向，而在试图最大化一个函数时，让参数持续往沿梯度方向使函数值上升最快的方向移动的方法，就称作 **梯度上升**(Gradient Ascent)。

同理，在最小化一个函数时，沿梯度方向下降最快的方向移动，则称为 **梯度下降**(Gradient Descent)。

## Gradient Ascent & Descent
梯度上升算法过程：
1. 随机初始化 $\displaystyle w$
2. 当 $\displaystyle w$ 未收敛时：
$$
w \gets w + \alpha \nabla_{w}f(w)
$$
对于梯度下降，更新函数改为 $\displaystyle w \gets w - \alpha \nabla_{w}f(w)$。

其中 $\displaystyle \alpha$ 表示 **学习率**，即梯度下降中每一步更新的步长。学习率大小需要适中，太大会导致无法收敛，太小则收敛太慢。通常学习率时动态变化的，初始时会使用较大的学习率，随后随着迭代次数增加，学习率逐步减小(Learning Rate Decay)。
