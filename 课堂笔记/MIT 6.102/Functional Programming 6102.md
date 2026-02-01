#mit6102 
## Abstracting out control
有时，我们需要对一组数据统一进行操作。传统的方式是组合 `for` 和 `if` 语句，对数据进行逐个判断与处理。

实际上，这些操作可以被抽象出来，形成更加简单直观的**设计模式**。

### Iterator
**迭代器**(Iterator) 是一个对象，可以一步步遍历一个序列，并逐个返回序列的元素。在 ts 中，迭代器 `Iterator<T>` 包含一个 `mutator` 方法 ` next() `，每次会返回一个对象 `{done: boolean, value?: T}`，并指向序列的下一个元素。

迭代器为序列数据结构提供了一个简单、通用的访问方式。

### Iterable
**可迭代**(Iterable)类型表示一个可以被迭代的元素序列。在 ts 中，`Iterable<T>` 接口表示一个可迭代类型，要求实现一个 `iterator` 方法，可以返回该序列的 `Iterator` 对象。