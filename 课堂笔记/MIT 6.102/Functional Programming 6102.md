#mit6102 
## Abstracting out control
有时，我们需要对一组数据统一进行操作。传统的方式是组合 `for` 和 `if` 语句，对数据进行逐个判断与处理。

实际上，这些操作可以被抽象出来，形成更加简单直观的**设计模式**。

### Iterator
**迭代器**(Iterator) 是一个对象，可以一步步遍历一个序列，并逐个返回序列的元素。在 ts 中，迭代器 `Iterator<T>` 包含一个 `mutator` 方法 ` next() `，每次会返回一个对象 `{done: boolean, value?: T}`，并指向序列的下一个元素。

迭代器为序列数据结构提供了一个简单、通用的访问方式。

### Iterable
**可迭代**(Iterable)类型表示一个可以被迭代的元素序列。在 ts 中，`Iterable<T>` 接口表示一个可迭代类型，要求实现一个 `iterator` 方法，可以返回该序列的 `Iterator` 对象。

### Generator
**生成器函数**(Generator Function) 是一种特殊的函数，调用时将返回一个 `Generator<T>` 对象（是 `Iterator<T>` 的子类型），并用 ` yield ` 语句生成序列中的下一个值。生成器函数的函数体只会在迭代器的 ` next ` 方法被调用时执行。

例如在 ts 中：
```ts
function* odds(): Generator<number> {
  for (let i = 1; true; i += 2) {
    yield i;
  }
}

function* addConstant(sequence: Iterable<number>, k: number): Generator<number> {
  for (const x of sequence) {
    yield x + k;
  }
}
```

`Generator` 既是 `Iterable`，也是 `Iterator`。
- 当调用 `Generator` 的 `iterator` 方法时，会直接返回自身。
- 可以作为 `iterator` 使用，调用其 `next` 方法，也只能遍历一次序列。

## Map/Filter/Reduce Abstraction
`map`、`filter`、`reduce` 是对控制语句的高层次的抽象：它们将整个序列视作一个元素进行整体操作。

### Map
**`map : Array<‍E> × (E → F) → Array<‍F>`**
将一个**一元函数**作用于序列中的每个元素，并以相同的顺序返回一个包含作用结果的新序列。

例：
```typescript
[1, 4, 9, 16].map(Math.sqrt);
```

有时我们不关心返回值，只是对序列中的每个元素执行 `mutator` 方法，可以使用 `forEach` ：
```ts
itemsToRemove.forEach(item => mySet.delete(item));
```

### Filter
**`filter : Array<‍E> × (E → boolean) → Array<‍E>`**
用一个映射到 `boolean` 的**一元函数**作为**谓词**测试每个元素，以原顺序保留且仅保留所有满足谓词的元素。

例：
```typescript
[1, 2, 3, 4].filter(x => x%2 === 1);
// returns [1, 3]

["x", "y", "2", "3", "a"].filter(s => "abcdefghijklmnopqrstuvwxyz".includes(s)));
// returns ["x", "y", "a"]

const isNonempty = (s: string) => s.length > 0;
["abc", "", "d"].filter(isNonempty);
// returns ["abc", "d"]
```

## Reduce
**`reduce : Array<‍E> × (F × E → F) × F → F `**
用一个**二元函数**将序列中的所有元素结合为一个标量元素。同时需要一个**初始值**。

```
result0 = init  
result1 = f(result0, arr[0])  
result2 = f(result1, arr[1])  
...  
resultn = f(resultn-1, arr[n-1])
```

在 ts 中，`reduce` 方法总是从左往右结合元素。也可以使用 `reduceRight` 方法逆序结合，使用方式一致。

