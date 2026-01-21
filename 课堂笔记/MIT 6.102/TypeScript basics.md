#mit6102 
## Mutatable & Reassignable
在 ts 中，字符串 (string) 是 immutable 对象，其内容不可以改变，只能基于其内容创建新的对象并重新赋值（reassign）。

也有些变量可以被设置为不可重赋值的 (unreassignable)，比如 `const n: number = 5;`，重新赋值时 ts 编译器会直接报错。

## Arrays, Maps, and Sets
和 Python 基本上只有语法上的不同，故只记录对比表。
### Arrays
|TypeScript|description|Python|
|---|---|---|
|`lst[2]`|get an element|`lst[2]`|
|`lst[2] = e`|set an element|`lst[2] = e`|
|`lst.length`|count the number of elements|`len(lst)`|
|`lst.push(e)`|add an element to the end|`lst.append(e)`|
|`lst.pop()`|remove an element from the end|`lst.pop()`|
|`if (lst.length === 0) ...`|test if the array is empty|`if not lst: ...`|
|`lst.includes(e)`|test if an element is in the array|`e in lst`|
|`lst.splice(i, 1)`|remove the element at index i|`del lst[i]`|
|`lst.slice(i, j)`|get a shallow copy of elements from index i to j (exclusive)|`lst[i:j]`|
### Maps
|TypeScript|description|Python|
|---|---|---|
|`map.set(key, val)`|add the mapping _key → val_|`map[key] = val`|
|`map.get(key)`|get the value for a key|`map[key]`|
|`map.has(key)`|test whether the map has a key|`key in map`|
|`map.delete(key)`|delete a mapping|`del map[key]`|
|`map.size`|count the number of mappings|`len(map)`|
### Sets
| TypeScript    | description                         | Python        |
| ------------- | ----------------------------------- | ------------- |
| `s.has(e)`    | test if the set contains an element | `e in s`      |
| `s.add(e)`    | add an element                      | `s.add(e)`    |
| `s.delete(e)` | remove an element                   | `s.remove(e)` |
| `s.size`      | count the number of elements        | `len(s)`      |
## Literals
```ts
let letters: Array<string> = ["a", "b", "c"];
```

```ts
let fruits: Map<string, number> = new Map([
    ["apple", 5],
    ["banana", 7],
]);
```

## Generics
泛型语法和 C++ 比较相似：
```ts
let cities: Array<string>;        // an Array of strings
let numbers: Set<number>;         // a Set of numbers
let turtles: Map<string, Turtle>; // a Map with string keys and Turtle values
```

声明 Array 的语法糖（和 cpp 更像了=/，只不过 cpp 里数组和 Vector 确实不是一回事）：
```ts
let cities: string[]; // also an Array of strings
```

## Iteration
和 Python 的 `for x in arr` 有点像，虽然 ts 也有 `const x in arr` 的语法，但行为与 `of` 不一样，不应在遍历 Array/Set/Map 时使用。
```ts
for (const city of cities) {
    console.log(city);
}

for (const num of numbers) {
    console.log(num);
}
```

```ts
for (const key of turtles.keys()) {
    console.log(key + ": " + turtles.get(key));
}

for (const value of turtles.values()) {
    console.log(value); // prints each Turtle
}

for (const [key, value] of turtles.entries()) {
    console.log(key + ": " + value);
}
```

注意：不应在遍历过程中修改所遍历的对象，尤其是在删除时。这可能会导致意料之外的结果。

使用下标遍历：
```ts
for (let ii = 0; ii < arr.length; ii++) { // 纯正C语言口音
	...
}
```

实际上这在 ts 中并没有那么必要，如果需要用到下标，可以使用以下语法：
```ts
for (const [ii, x] of arr.entries()) {
	...
}
```
就如同将 Array 视为 Map，将 index 视为 key 一样。

```ts
const prices: Map<string, n	umber> = new Map();
prices.set("apple", 5);
prices.get("apple")   // returns 5
prices.get("banana")  // returns undefined
```

## Variable declaration
ts 支持自动推断类型（帅 auto 帅）
```ts
let x = 5; // infers that x: number
let name = "Frodo"; // infers that name: string
```

泛型初始化时，可以自动推断声明类型，也可以自动推断 initializer，至少有一个完整类型即可。
```ts
// full type in the declaration; initializer type inferred
let numbers: Array<string> = [];
let map: Map<string, number> = new Map();

// full type in the initializer; declaration type inferred
let numbers = new Array<string>();
let map = new Map<string, number>();
```

无法自动推断时，将导致静态类型错误：
```ts
let numbers = [];      // NO: what is the element type of the array?
let ages = new Set();  // NO: what is the element type of the set?
let map = new Map();   // NO: what are the types of the keys and values?
```

## let vs. var
`var` 的作用域是整个函数，而 `let` 的作用域是当前代码块（基本上等同于由 `{}` 包裹的区域）。一般而言，我们只使用 `let`。

## initialization check
在程序运行之前，ts 会静态检查变量在访问之前是否一定会被赋值，例：
```ts
let a = 5;
let b: number;
if (a > 10) {
    b = 2;
} else {
    // b = 4;
}
b *= 3;
```
以上程序在编译阶段会报错，因为 b 有可能在 `b *= 3` 前没有被赋值。

## Object literals and Object types
ts 可以用类似于 Python 的 dictionary 语法创建一个对象字面量：
```ts
{ x: 5, y: -2 }
```

该对象的静态类型是 `{ x: number, y: number }`：
```ts
let point: { x: number, y: number } = { x: 5, y: 2 };
```

一个只是一些字段的集合，而不含任何方法的对象类型，称为“记录”(record)，在 C 语言中也叫结构体 (struct)。简单的记录类型可用于方便地返回多个值：
```ts
function integerDivision(a: number, b: number): { quotient: number, remainder: number } {
    return {
        quotient: ...,  // result of dividing a by b
        remainder: ..., // remainder after dividing a by b
    };
}
```

对象字面量还支持解构语法：
```ts
const { quotient, remainder } = integerDivision(23, 7);
```