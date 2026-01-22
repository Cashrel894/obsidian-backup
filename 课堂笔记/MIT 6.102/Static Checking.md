#mit6102 
## Checking
编程语言的自动检查功能通常分为以下三种：
- **静态检查**：在程序运行之前检查问题。
	- 包括语法错误、名称错误、类型错误等。
- **动态检查**：在程序运行时检查问题。
	- 包括非法参数值（如除以 0 错误）、非法类型转换、数组越界等。

然而在 TypeScript 中，通常不进行动态检查，例如：
- 在 Array 下标越界或在 Map 中找不到 key 时，会返回 `undefined`。
- 除以 0 时，会返回 `Number.POSITIVE_INFINITY` （正数）、 `Number.NEGATIVE_INFINITY`（负数）或 `NaN`（0）。

## Dynamic Checking Traps
尽管 ts 长于静态检查，但其动态检查部分由 js 的部分负责，而这一块 js 做的很糟糕。

### Number
`number` 类型的存储方式是双精度浮点数，这也就意味着其也存在 `double` 类型的各种问题：
- 整数的存储精度有限，只有 `[Number.MIN_SAFE_INTEGER(-2^53), Number.MAX_SAFE_INTEGER(2^53)]` 范围内的正数能够被精确存储。
- 有各种特殊值，即 `Number.NaN` (Not a Number)、`Number.POSITIVE_INFINITY` (Infinity) 和 `Number.NEGATIVE_INFINITY` (-Infinity)
- 存在溢出行为，如大于 `Number.MAX_VALUE` (约 10^308)的正数会变成 Infinity，小于 `Number.MIN_VALUE` (约 10^-324)的正数会变成 0。

## Documenting assumptions
在 ts 中，为函数编写文档时，我们应当记录的是有关函数参数的**假设**，而无需记录其**类型**，因为类型已经在函数声明中体现、并受到 ts 编译器检查，因而无需特别指出。

我们要做的是规范函数的输入，并指出返回值的性质，来指导使用者如何使用该函数。

例：
```ts
/**
 * Compute a hailstone sequence.
 * @param n  starting number for sequence.  Assumes n > 0.
 * @returns hailstone sequence starting with n and ending with 1.
 */
function hailstoneSequence(n: number): Array<number> {
    let array: Array<number> = [];
    while (n !== 1) {
        array.push(n);
        if (n % 2 === 0) {
            n = n / 2;
        } else {
            n = 3 * n + 1;
        }
    }
    array.push(n);
    return array;
}
```