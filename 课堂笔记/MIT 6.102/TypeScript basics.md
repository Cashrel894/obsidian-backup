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


```ts
let cities: string[]; // also an Array of strings
```
