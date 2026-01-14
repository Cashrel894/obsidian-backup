#EECS70 
***定义：由若干确定的对象（或元素/成员）组成的整体。***

对于集合 A，若 x 是 A 的元素，数学上可以写作 $\displaystyle x \in A$ .

相似地，若 y 不是 A 的元素，数学上可以写作 $\displaystyle y\not\in A$.

如果集合 A 和 B 拥有相同的元素，则写作 $\displaystyle A=B$.

集合并**不关心**其内部元素的排列顺序。

有时，更复杂的集合可以被有多种表示方法：例如有理数集 $\displaystyle \mathbb{Q}$，也可以被写成 $\displaystyle \left\{ \frac{a}{b} \mid a,b\in \mathbb{Z},b\neq 0 \right\}$.

## Cardinality
集合的**势**（Cardinality），用于描述一个集合所含元素的多少，A 的势记作 $\displaystyle |A|$。一个势为 0 的集合被称为**空集**，记作 $\displaystyle \emptyset$。集合也可以含有无限多的元素，这时集合的势就不能用常数表示了。

## Subsets and Proper Subsets
如果 A 的每个元素也是 B 的元素，那么 A 就是 B 的**子集**，记作 $\displaystyle A\subseteq B$，也可以写作 $\displaystyle B\supseteq A$。

若此时 B 至少包含一个不属于 A 的元素，那么 A 就是 B 的**真子集**，记作 $\displaystyle A\subset B$。

## Intersections and Unions
A 和 B 的**交集**（intersection），指由所有同时属于 A 和 B 的所有元素组成的集合，记作 $\displaystyle A\cap B$。若 $\displaystyle A\cap B=\emptyset$，那么称这两个集合是**不相交的**（disjoint）。

A 和 B 的**并集**（union），指由所有属于 A 或属于 B 的所有元素组成的集合，记作 $\displaystyle A\cup B$。

## Complements
对于集合 A 和 B，那么 **A 在 B 中的相对补**(relative complement)或 **B 与 A 的集合差**(set difference) 就是由全体属于 B 但不属于 A 的元素组成的集合，记作 $\displaystyle B-A$ 或 $\displaystyle B \setminus A$：$\displaystyle B \setminus A = \{ x \in B \mid x \not\in A \}$。

性质：
- $\displaystyle A\setminus B=\emptyset$
- $\displaystyle A\setminus \emptyset=A$
- $\displaystyle \emptyset \setminus A=\emptyset$

## Products and Power Sets
对于集合 A 与 B，A 和 B 的**笛卡尔积**（也称为**叉积**）指：由所有以 A 中元素为第一个成员、以 B 中元素为第二个成员，所有这样的元素对组成的集合，记作 $\displaystyle A\times B$：$\displaystyle A\times B=\{ \left( a, b \right)\mid a\in A, b \in B \}$。

对于集合 A，A 的**幂集**(power set) 指由 A 的所有子集组成的集合，记作 $\displaystyle \mathcal{P}(A)$：$\displaystyle \mathcal{P}(A) = \{ B \mid B \subseteq A\}$。

