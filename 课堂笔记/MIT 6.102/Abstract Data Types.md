#mit6102 
## What Abstraction Means
**抽象数据结构**是软件工程中一种重要的思想。它包含以下内涵：
- **抽象**：用简单、高层次的接口省略或隐藏低层次的细节。
- **模块化**：将软件系统划分为多个组件或模块，每个模块都可以单独设计、实现、测试和复用。
- **封装**：为模块构建起一道防火墙，隔离客户端与内部实现，内外正确性互不影响。
- **信息隐藏**：将模块的实现细节隐藏起来，使得内部实现可以在不影响客户端使用的情况下任意修改。
- **关注点分离**：一个关注点只交给一个模块负责，而非分布到各个模块。

数据抽象的核心思想：**一种类型由其参与的操作决定**。

## Classifying Types and Operations
一个抽象数据类型的操作可以被分为如下几类：
- **Creator**：创建该类型的新对象，可以接受其他类型的参数，但不会接受本类型的对象作为参数。
	- 通常用**构造方法**实现。
	- 有时使用静态方法或独立的函数实现，这样的方法/函数被称为 **Factory**。
- **Producer**：使用一个或多个本类型的对象，创建该类型的新对象。
	- 例如 `string` 类的 ` concat ` 方法。
	- 又如 `number` 类的各种四则运算方法。
- **Observer**：接受本类型的对象，返回其他类型的对象。
	- 如 `Map` 的 `size` 方法。
- **Mutator**：修改对象。

## An Abstract Type is Defined by its Operations
规范是单个函数的防火墙，而相似地，抽象数据类型是一系列**相关函数**（即类型参与的操作）和**数据**的防火墙。实现抽象数据类型的具体方式，就称为**表示**(Representation)。

具体而言，*表示*包含了一个类的所有字段，以及描述这些字段的假设或要求。

## Designing an Abstract Type
设计一个抽象数据结构涉及以下核心原则：
- 只包含**少量**、**简单**的操作，但可以通过操作的组合提供强大的功能。
- 每个操作都应提供**一致的**、**良定义的**行为，能够覆盖绝大多数应用情况。
- 提供的操作应当**充分地**覆盖客户端可能需要的行为。
- 同一个抽象类型内不应该混杂**通用**(General-purpose)和**领域特定**(Domain-specfic)的功能。
	- 例如，表示纸牌牌组的 `Deck` 类的 `add` 方法不应该接受 `int` 等类型的对象，也不应该在 `Array` 类加入 `dealCards` 等领域特定的方法。

## Representation independence
一个好的抽象数据类型应当是具有**表示独立性**(Representation Independence) 的。
- 也就是说，抽象数据类型的使用应当是**独立于**它的表示的。因此，更改表示并不会影响它的使用。
```ts
/** MyString represents an immutable sequence of characters. */
class MyString { 

    //////////////////// Example creator operation ///////////////
    /**
     * @param s 
     * @returns MyString representing the sequence of characters in s
     */
    public constructor(s: string) { ... }

    //////////////////// Example observer operations ///////////////
    /**
     * @returns number of characters in this string
     */
    public length(): number { ... }

    /**
     * @param i character position (requires 0 <= i < string length)
     * @returns character at position i
     */
    public charAt(i: number): string { ... }

    //////////////////// Example producer operation ///////////////
    /** 
     * Get the substring between start (inclusive) and end (exclusive).
     * @param start starting index
     * @param end ending index.  Requires 0 <= start <= end <= string length.
     * @returns string consisting of charAt(start)...charAt(end-1)
     */
    public substring(start: number, end: number): MyString { ... }

    /////// no mutator operations (why not?)
}
```

而大部分编程语言的**访问控制**机制允许我们将表示设置为 private，不允许外界访问，从而允许编译器**静态检查**表示独立性。

## ADT in TypeScript

| ADT concept                                                                                                    | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@ways_do_typescript)Ways to do it in TypeScript | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@examples)Examples                                                                                                                                                                                                      |     |
| -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@abstract_data_type_1)Abstract data type | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@class_1)Class                                  | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@date)[`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)                                                                                                                   |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@interface_class-es_1)Interface + class(es)     | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@arraylike_array)[`ArrayLike`](https://typhonjs-typedoc.github.io/ts-lib-docs/2023/esm/interfaces/ArrayLike.html) and [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@enum_2)Enum                                    | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@pencolor)[`PenColor`](https://web.mit.edu/6.102/www/sp25/psets/ps0/doc/enums/turtle.PenColor.html)                                                                                                                     |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@creator_operation)Creator operation     | Constructor                                                                                                           | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@array_1)[`Array()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)                                                                                                      |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@static_factory_method)Static (factory) method  | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@array-of)[`Array.of()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)                                                                                                     |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@constant_3)Constant                            | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@number-positive_infinity)[`Number.POSITIVE_INFINITY`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY)                                                       |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@observer_operation)Observer operation   | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@instance_method_0)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@string-charat)[`String.charAt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)                                                                                      |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@static_method_0)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@object-entries)[`Object.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)                                                                                   |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@producer_operation)Producer operation   | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@instance_method_1)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@string-trim)[`String.trim()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)                                                                                            |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@static_method_1)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@math-floor)[`Math.floor()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)                                                                                               |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@mutator_operation)Mutator operation     | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@instance_method_2)Instance method              | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@array-push)[`Array.push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)                                                                                               |     |
|                                                                                                                | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@static_method_2)Static method                  | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@object-assign)[`Object.assign()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)                                                                                      |     |
| [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@representation_5)Representation         | [](https://web.mit.edu/6.102/www/sp25/classes/06-abstract-data-types/#@private_fields)`private` fields                |                                                                                                                                                                                                                                                                                               |     |

## Testing an Abstract Data Type
测试一个抽象数据类型的核心思想和 [[Testing 6102]] 是一致的，我们只需要分别测试它的所有操作即可。

只不过，对某一操作的测试往往会不可避免地涉及到另一个操作。例如，测试 Creator、Producer 或 Mutator 都需要调用 Observer；测试 Observer 又需要 Creator 来创建对象。

```ts
// testing strategy for each operation of MyString:
//
// constructor, length(), charAt(), substring():
//    partition on string length: 0, 1, >1
// length(), charAt(), substring():
//    partition on this: produced by constructor, produced by substring()
// charAt(): 
//    partition on i=0, 0<i<len-1, i=len-1
// substring():
//    partition on start=0, 0<start<len, start=len
//    partition on end=0, 0<end<len, end=len
//    partition on end-start: 0, >0
```

注意，我们在测试一个抽象数据类型时，我们应当基于 `this` 的**抽象状态**划分输入空间，而不是它的具体表示。我们应该以一个**客户端**的视角进行划分和测试。