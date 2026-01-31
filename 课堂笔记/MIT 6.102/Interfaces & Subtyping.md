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

## Why Interface?
- 作为文档供客户端使用（以及编译器检查）。
- 允许性能权衡。不同的实现可能显示出不同的性能特征，方便客户端进行权衡使用。
- 接口可能含有有意欠定的规范，允许实现者自由决定，客户端也可以根据需要选择更强的实现。
- ……

## Subclassing
**子类**(Subclass) 是比**子类型**更强的概念，它会继承其父类的所有的私有表示、方法实现。

然而，相比子类型而言，子类通常安全性较差，具体体现在：
- 父类和子类之间的表示泄露。
- 父类和子类之间的表示依赖。
- 父类和子类可能意外相互破坏表示不变量。

## Overriding and Dynamic Dispatch
子类可以**覆写**(Override) 父类的方法，而当某个对象的方法被调用时，会发生以下步骤：
1. 检查对象所属的**静态类型**，若该静态类型没有目标方法，静态检查将抛出错误。
2. 调用对象**动态类型**的目标方法，可能被子类覆写。
这种检查和方法查找模式，称为**动态调度**(Dynamic Dispatch)。

## Generic Types
**泛型**(Generic Type)：规范中需要使用到类型作为参数的类型，例如 `Set<T>` 等。

泛型可以被视为一些相似类型的集合，这些类型依照尖括号中的类型参数而定。

## Getters & Setters
在 ts 中，可以定义 getter 和 setter 方法，将对私有字段的访问和修改作为接口主动暴露出去，在客户端看来是对字段直接进行访问与修改，但内部可能是通过 getter 或 setter 方法实现的。
```ts
class FastMyString implements MyString {

    public get length(): number {
        return this.end - this.start;
    }

    // ... 
}
```

```ts
class SimpleMyString implements MyString {

    public readonly length;

    // ...
}
```

## ADTs in TypeScript
| ADT concept                                                                                                   | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@ways_do_typescript)Ways to do it in TypeScript | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@examples)Examples                                                                                                                                                                                                      |     |
| ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@abstract_data_type)Abstract data type | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@class_7)Class                                  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@date)[`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)                                                                                                                   |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@interface_class-es)Interface + class(es)       | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@arraylike_array)[`ArrayLike`](https://typhonjs-typedoc.github.io/ts-lib-docs/2023/esm/interfaces/ArrayLike.html) and [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@enum_4)Enum                                    | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@pencolor)[`PenColor`](https://web.mit.edu/6.102/www/sp25/psets/ps0/doc/enums/turtle.PenColor.html)                                                                                                                     |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@creator_operation)Creator operation   | Constructor                                                                                                            | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@array)[`Array()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)                                                                                                        |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@static_factory_method)Static (factory) method  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@array-of)[`Array.of()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)                                                                                                     |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@constant)Constant                              | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@number-positive_infinity)[`Number.POSITIVE_INFINITY`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY)                                                       |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@observer_operation)Observer operation | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@instance_method_0)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@string-charat)[`String.charAt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)                                                                                      |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@static_method_0)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@object-entries)[`Object.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)                                                                                   |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@getter)Getter                                  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@map-size)[`Map.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size)                                                                                                       |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@producer_operation)Producer operation | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@instance_method_1)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@string-trim)[`String.trim()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)                                                                                            |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@static_method_1)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@math-floor)[`Math.floor()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)                                                                                               |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@mutator_operation)Mutator operation   | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@instance_method_2)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@array-push)[`Array.push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)                                                                                               |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@static_method_2)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@object-assign)[`Object.assign()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)                                                                                      |     |
|                                                                                                               | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@setter)Setter                                  |                                                                                                                                                                                                                                                                                                |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@representation_1)Representation       | [](https://web.mit.edu/6.102/www/sp25/classes/08-interfaces-subtyping/#@private_fields)`private` fields                |                                                                                                                                                                                                                                                                                                |     |
