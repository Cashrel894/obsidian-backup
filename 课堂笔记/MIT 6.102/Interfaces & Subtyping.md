#mit6102 
## Interface
在 ts 中，我们可以把方法签名和规范独立地放在 `interface` 中，让具体实现接口的类 `implements` 该 `interface`，进而实现抽象与表示之间的分离。
```ts
interface Curve {
	/** ... */
	contains(x: number, y: number): boolean;
}

class ArrayCurve implements Curve {
	public contains(x: number, y: number): boolean {
		...
	}
}
```

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

## Factory
由于接口本身无法实例化对象，创建对象时必须指定具体实现该接口的类，而这就打破了抽象和表示之间的壁垒。

因此，我们需要定义**工厂函数**(Factory Function)，进而将表示类隐藏起来：
```ts
/**
 * @param s 
 * @returns MyString representing the sequence of characters in s
 */
function makeMyString(s: string): MyString {
    return new FastMyString(s);
}
```

