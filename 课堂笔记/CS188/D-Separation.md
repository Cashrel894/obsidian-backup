#CS188 
## Casual Chains
在贝叶斯网络中，像这样的三个结点被称为 **因果链**(Casual Chains)：
![[Pasted image 20260311155411.png]]

其表达了三个变量联合分布的性质：
$$
P(x,y,z)=P(z|y)P(y|x)P(x)
$$
注意，$\displaystyle X$ 和 $\displaystyle Z$ 未必是独立的。

但是，在给定 $\displaystyle Y$ 的情况下，可以说 $\displaystyle X$ 与 $\displaystyle Z$ 是相互独立的。证明如下：
$$
P(X|Z,y)=\frac{P(X,Z,y)}{P(Z,y)}=\frac{P(Z|y)P(y|X)P(X)}{P(Z,y)}=P(X,y)
$$
## Common Cause
这样的结构则称为 **共因**(Common Cause)：
![[Pasted image 20260311160355.png]]

表达了以下概率关系：
$$
P(x,y,z)=P(x|y)P(z|y)P(y)
$$

同样，当 $\displaystyle Y$ 给定时，必有 $\displaystyle X$ 与 $\displaystyle Z$ 独立：
$$
P(X|Z,y)=\frac{P(X,Z,y)}{P(Z,y)}=\frac{P(X|y)P(Z|y)P(y)}{P(Z,y)}=P(X|y)
$$

## Common Effect
这样的结构则被称为 **共果**(Common Effect)：
![[Pasted image 20260311160736.png]]

表达这样的概率关系：
$$
P(x,y,z)=P(y|x,z)P(x)P(z)
$$


