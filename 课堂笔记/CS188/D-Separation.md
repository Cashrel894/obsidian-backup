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

此时，当 $\displaystyle Y$ *未给定*时，$\displaystyle X$ 与 $\displaystyle Z$ 独立，否则不独立。

## General Case, and D-separation
**D 分离**(D-Separation)：在贝叶斯网络中，若在给定 $\displaystyle \{ Z_{1},\dots Z_{k} \}$ 的情况下，$\displaystyle X$ 与 $\displaystyle Y$ 相互独立，那么称 $\displaystyle Z_{1},\dots,Z_{k}$ D-分离 $\displaystyle X$ 与 $\displaystyle Y$。

以下是用于判断 D-分离的算法步骤：
1. 在图中标记 $\displaystyle {Z_{1},\dots Z_{k}}$。
2. 枚举所有 $\displaystyle X$ 到 $\displaystyle Y$ 的无向路径。
3. 对于每条路径：
	1. 枚举所有连续的三个结点，根据上面三种情况判断，得出两边结点都不独立的情况下，称该路径 **D-连接**(D-Connect) $\displaystyle X$ 和 $\displaystyle Y$，$\displaystyle X$ 和 $\displaystyle Y$ 不独立。
4. 如果没有路径 D-连接 $\displaystyle X$ 和 $\displaystyle Y$，那么 $\displaystyle X$ 和 $\displaystyle Y$ 独立。
