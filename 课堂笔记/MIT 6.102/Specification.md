#mit6102 
## Behavioral Equivalence
**行为等价性**(Behavioral Equivalence) 是指，能否在不影响正确性的情况下替换具体实现。

行为等价性是相对于使用该模块的客户端而言的。只要客户端**遵循规范**使用模块的输入/输出，行为等价的实现就没有任何行为上的区别。

规范可以视为客户端侧与具体实现侧的**防火墙**：![[Pasted image 20260127132436.png]]

规范的存在，使得客户端可以在不关心具体实现的情况下，任意使用该模块；模块的实现也可以在不关心客户端如何使用的情况下，任意替换其实现，只要双方都遵守规范的约束。换句话说，规范实现了客户端与实现者之间的**解耦**。

## Specification Structure
一个规范包括以下几个部分：
- 函数签名 (Function Signature)，指明函数名、参数类型和返回类型。
- 一个 requires 块，描述对参数的额外约束。
- 一个 effects 块，描述函数的返回值、可能引发的异常和其他副作用。

以上这些部分共同决定了函数的**前置条件**(Precondition)和**后置条件**(Postcondition)。
- 前置条件：是对客户端的约束条件，限制了客户端在调用函数时的状态。
- 后置条件：是对实现者的约束条件，限制了**在满足前置条件的前提下**，调用函数应该导致的程序状态。

注意，当前置条件不满足时，实现者**没有义务**满足后置条件。然而，根据 Fail Fast 原则，更好的选择时当前置条件不满足时直接抛出异常，从而快速暴露客户端侧的 Bug。

```ts
/**
 * Find a value in an array.
 * @param arr array to search, requires that val occurs exactly once
 *            in arr
 * @param val value to search for
 * @returns index i such that arr[i] = val
 */
function find(arr: Array<number>, val: number): number
```

