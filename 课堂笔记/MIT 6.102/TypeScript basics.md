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
const prices: Map<string, number> = new Map();
prices.set("apple", 5);
prices.get("apple")   // returns 5
prices.get("banana")  // returns undefined
```