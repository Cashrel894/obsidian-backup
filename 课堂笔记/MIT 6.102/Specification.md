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

在 ts 中，分别用 `@param` 和 `@returns` 描述参数和返回值。
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

## Specifications for Mutating Functions
有时，函数不仅会返回某些值，还可能产生一些副作用，如：
```ts
addAll(array1: Array<string>, array2: Array<string>): boolean
```
```
requires: `array1` and `array2` are not the same object
effects: modifies `array1` by adding the elements of `array2` to the end of it, and returns true if and only if `array1` changed as a result of call
```
注意：除非明确说明，应该默认模块没有任何副作用，包括修改 mutable 的参数等。

## Exceptions
**异常**(Exception) 也可以是函数在某些特定情况下的输出，用于处理可能的意外情况。在 ts 中，模块抛出的异常可以用 `@throws` 块描述。

异常可以用于标识 Bug 的存在，也可以标识可能的错误的来源，使得调用者可以捕获并响应这些错误。例如：
```ts
/**
 * Compute the integer square root.
 * @param x integer value to take square root of
 * @returns square root of x
 * @throws NotPerfectSquareError if x is not a perfect square
 */
function integerSquareRoot(x: number): number
```

```ts
/**
 * If the array1 !== array2, adds the elements of array2 to the end of array1.
 * @returns true if array1 changed as a result
 * @throws AliasingError if array1 === array2
 */
function addAll(array1: Array<string>, array2: Array<string>): boolean
```

**注意**：`throws` 只应描述**前置条件成立的前提下**的可能错误，标识 Bug 的异常不应该在规范中被描述，因为抛出异常只是前置条件不成立时的诸多可能行为的一种。

## Special Values
另一种处理意外情况的方法是返回一个特殊值，如 `undefined`。
```ts
/**
 * Compute the integer square root.
 * @param x integer value to take square root of
 * @returns square root of x if x is a perfect square, undefined otherwise
 */
function integerSquareRoot(x: number): number|undefined
```

此时，需要客户端自行处理特殊值，例如 `assert(ret !== undefined)` 等。