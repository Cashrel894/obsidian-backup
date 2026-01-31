#mit6102 
## Interface

## Subtypes
所谓**子类型**(Subtype)，简单来说就是某个**父类型**(Supertype) 的子集。即子类型的规范总是**强于**（或强度相等）其父类型的规范。

在 TypeScript 中，可以使用 `extend` 语法扩展特定接口，来实现子类型：
```ts
interface ReversibleArrayLike<Element> extends ArrayLike<Element> {
    // inherits signatures and specs of existing ArrayLike operations, and adds new ones:

    /** Reverse this array, mutating it in place. */
    reverse(): void;
}
```

## Structual Subtyping
在 ts 中，如果类型 B 实现了类型 A 的接口所要求的所有操作，那么 B 就被视作 A 的**形式子类型**(Structual Subtype)。此时，ts 会将 B 视作 A 的子类型处理。

这和**鸭子类型**非常相似，我们只关心一个类型**能做什么**，而不在乎**它是什么**。

