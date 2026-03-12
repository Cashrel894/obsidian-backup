#CS188 
理性代理必须遵从 **效益最大化原则** (Principle of Maximum Utility)，即它们总是选取使得期望效益最大的行动。

然而，这一切的前提是代理必须有着 **理性偏好**(Rational Preferences)。

所谓的 **偏好**，可以进行如下数学定义：
- 如果代理偏好 A 胜过 B，写作 $\displaystyle A \succ B$。
- 如果 A 和 B 对代理而言并无区别，那么 $\displaystyle A \sim B$。
- **彩票**（Lottery）是指以不同概率得到不同结果的情形，如果代理有 p 的概率得到 A，(1-p) 的概率得到 B，则写作：$\displaystyle L=[p,A;(1-p),B]$。

而所谓**理性偏好**，必须满足 **理性五大公理**(The Five Axioms of Rationality)：
- **可序性**(Orderabiltiy)：$\displaystyle (A \succ B)\lor(B \succ A) \lor (A \sim B)$。
- **传递性**(Transitivity)：$\displaystyle (A \succ B)\land (B \succ C)\implies(A \succ C)$
- **连续性**(Continuity)：$\displaystyle A \succ B \succ C \implies \exists p[p, A;(1-p),C]\sim B$。
- **可替换性**(Substitutability)：$\displaystyle A \sim B\implies[p,A;(1-p),C]\sim[p,B;(1-p),C]$。
- **单调性**(Monotonicity)：$\displaystyle A \succ B \implies (p \geq q) \iff[p,A;(1-p),B]\succ [q,A;(1-q),B]$。
如果一个代理的偏好同时满足以上公理，那么此时代理的行为将 **等价于最大化期望效益**。这也意味着，存在一个实数值函数 $\displaystyle U$，满足：
$$
U(A)\geq U(B) \iff A\geq B
$$
$$
U([p_{1},S_{1};\dots ;p_{n},S_{n}])=\sum_{i}p_{i}U(S_{i})
$$
而同一种偏好可能存在不同的效益函数 $\displaystyle U$，从而导致代理对于 *风险管理* 有着不同的行为，主要包含 *风险中立型*(Risk Neutral)、*风险规避型*(Risk Averse)、*风险追求型*(Risk Seeking)。