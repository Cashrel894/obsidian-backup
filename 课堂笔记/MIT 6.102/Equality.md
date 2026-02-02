#mit6102 
在软件开发中，对象之间的相等性检查非常重要，尤其是在**编写测试**时。
## Equivalence Relation
类型 $\displaystyle T$ 的**等价关系** $\displaystyle E\subseteq T\times T$ ，满足：
- **自反性**(Reflexive)：$\displaystyle (t, t) \text{ for all } t\in T$
- **对称性**(Symmetric)：$\displaystyle (t, u) \in E \implies (u, t) \in E$
- **传递性**(Transitive)：$\displaystyle (t, u) \in E \wedge (u, v) \in E \implies (t, v) \in E$

在定义等价关系时，务必确保运算满足以上三个条件，否则很可能导致意料之外的问题。

## Equality of Immutable Types
不可修改对象的相等性可以有以下两种定义方式：
- 使用**抽象函数**。对象 a 和 b 相等当且仅当 `AF(a)=AF(b)`
- 调用方法、观察结果。若对象 a 和 b 调用任何方法的结果都相同，那么二者相等。

## Equality of Mutable Types
对于可修改类型，需要考虑 `mutator` 的作用。有以下两种定义方式：
- 观察性相等：在调用 `mutator` 修改的情况下，两个对象调用 `observer` 或 `producer` 的结果总是相同。
- 行为性相等：即使只在其中一个对象上调用 `mutator` 改变其状态，这两个对象的行为也表现不出任何区别。

## Hash Functions
在 `Set`、` Map ` 等数据结构往往使用**哈希表**实现，而哈希表的原理是依据插入对象的**哈希值**放入对应的列表中，每次查询时只需要找到哈希值对应的列表，暴力搜索比较即可。

这也就意味着，在定义两个对象的相等性之后，我们也**必须**重定义其哈希值，保证相等的对象其哈希值也相等，防止 `Set` 和 `Map` 出现错误行为。

同时，这也说明**观察性相等的可修改对象不应该成为 `Set` 或 `Map` 的键**。因为已经被插入哈希表中的元素如果被修改，其哈希值也会随之改变，但它在哈希表中的位置不会更改，进而导致错误。不过，行为性相等的可修改对象没有这种问题，因为无论如何修改，其哈希值仍不会改变。事实上，Python 就只允许不可修改的对象作为字典的键。