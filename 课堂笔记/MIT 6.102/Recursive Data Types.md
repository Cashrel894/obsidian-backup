#mit6102 
## Example: Immutable Lists
`ImList<Element>` 类型有四个基本操作：
![[Pasted image 20260206150124.png]]

## Recursive Data Type Definition
抽象数据类型 `ImList<Element>` 由两个实际类组成：`Empty` 和 `Cons`，共同组成一个**递归数据类型**(Recursive Data Type)。

`ImList<Element>` 的**数据类型定义**(Data Type Definition)如下：
```
ImList<Element> = Empty + Cons(first: Element, rest: ImList<Element>)
```

通过这种方式，可以清晰展现 ADT 的递归性质。

正式来讲，数据类型定义包含：
- 等号左侧是**抽象数据类型**，右侧是其**表示**。
- 表示包含该数据类型的各个**变种**，用 `+` 等符号连接。
- 每个变种是一个包含若干字段的类名。
而所谓**递归数据类型定义**，是指等号右侧的定义包含了等号左侧的 ADT 本身的数据类型定义。

又如二叉树的数据类型定义：
```plaintext
Tree<Element> = Empty + Node(e: Element, left: Tree<Element>, right: Tree<Element>)
```

在函数式编程中，数据类型定义又被称为**代数数据类型**(Algebratic Data Type)。

