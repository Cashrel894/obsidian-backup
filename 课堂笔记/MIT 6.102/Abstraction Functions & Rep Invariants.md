#mit6102 
## Invariants
一个好的抽象数据结构，总是能保持自身的某些**不变量**(Invariant)。
- 不变量有多种形式，例如不可修改性 (Immutability)、变量类型、变量之间的关系等。

为了保持不变量，数据结构通常会保护自身的私有变量不被外界直接访问，并只允许一些良定义的操作与数据结构进行互动。

保持不变量的好处有：
- 容易对代码进行形式推理。
- 容易进行调试，可以根据不变性轻松地排除很多情况。
- 减少客户端的误用行为。

## Immutability
在设计不可修改的数据类型时，需要注意可修改对象的使用，例如：
- 若直接将外界传入的可修改对象的引用赋给内部变量，该对象有可能发生修改，造成**表示泄露**(Rep Exporsure)。
- 在向外界直接传出可修改对象时，外界对对象的修改可能影响对象的不可变性。

因此，`private` 的字段不一定意味着不可访问，一旦有操作泄露了可修改对象的引用，外界就有可能通过引用作出意外修改。

## Rep Invariants and Abstraction Functions
**抽象值**(Abstract Value) ：指**客户端视角**下数据类型所支持的值。
**表示值**(Rep Value)：指**实现者视角**下用于实现抽象值的实际对象。

例如用 `string` 作为表示值，来实现抽象值 `CharSet`：
![[Pasted image 20260130134724.png]]

可以观察到两者之间的关系存在以下特点：
- 每个抽象值都**至少存在一个**映射到它的表示值。
- 多个表示值可以映射到一个抽象值。
- 并不是所有表示值都有一个抽象值与之对应。

为了进一步刻画两者之间的关联，我们引入以下两个概念：
1. **抽象函数**(Abstraction Function)：表示值到抽象值的一个映射，即 `AF: R -> A`。注意到抽象函数一定是**满射的**，但不一定单射，且通常定义域非全集。
2. **表示不变量**(Rep Invariant)：表示值到布尔值的一个映射，即 `RI: R -> boolean`。换句话说，表示不变量是对表示值的一个**谓词**，且满足 `RI(r)` 当且仅当 `r` 属于 `AF` 的定义域，即存在抽象值与 `r` 对应。

抽象函数和表示不变量都应该记录在代码注释中，例如：
```ts
class CharSet {
    private s: string;
    // Rep invariant:
    //   s contains no repeated characters
    // Abstraction function:
    //   AF(s) = {s[i] | 0 <= i < s.length}
    ...
}
```
这样的注释**非常重要**，特别是当 `AF` 并不明确，或者需要进行团队合作的时候。

同时，在代码运行时，也有必要时刻检查（最好在每个方法的最后都进行检查，只要不严重影响程序效率）自身表示是否符合**表示不变量**，并在不符合时**抛出错误**，以尽快发现问题根源。

## No Null Values in the Rep
之前提过，函数的前置和后置条件默认数据非 null。**表示不变量也同样需要强调非 null**。表示中涉及到的任何引用，包括 `Array` 等数据结构中，都默认不含 null / undefined 引用。

## Benevolent Side-effects
一个类型是**不可修改的**当且仅当其**抽象值**不可修改。然而，我们依然可以修改表示值，只要其所对应的抽象值不变即可，这也被称为“善意的副作用”。

例如，对于有理数类：
```ts
class RatNum {

    private numerator: bigint;
    private denominator: bigint;

    // Rep invariant:
    //   denominator != 0

    // Abstraction function:
    //   AF(numerator, denominator) = numerator/denominator

    /**
     * Make a new RatNum = (n / d).
     * @param n numerator
     * @param d denominator
     * @throws Error if d = 0
     */
     public constructor(n: bigint, d: bigint) {
        if (d === 0n) throw new Error("denominator is zero");
        this.numerator = n;
        this.denominator = d;
        checkRep();
    }

    /**
	 * @returns a string representation of this rational number
	 */
	public toString(): string {
	    const g = gcd(this.numerator, this.denominator);
	    this.numerator /= g;
	    this.denominator /= g;
	    if (this.denominator < 0n) {
	        this.numerator = -this.numerator;
	        this.denominator = -this.denominator;
	    }
	    checkRep();
	    return (this.denominator > 1n) ? (this.numerator + "/" + this.denominator)
	                             : (this.numerator + "");
	}
}
```

尽管在 `toString` 方法修改了字段的值，但约分并不改变其所对应的有理数值，因此不会改变类型的不可修改性。

## Documenting the AF, RI, and safety from rep exposure
记录 AF 和 RI 时应力求**精确**，而不是仅仅给出一个宽泛的描述，而是要定义一个严格的、无歧义的映射规则。

同时，也务必记录防范**表示泄露**的具体措施，例如：
```ts
// Safety from rep exposure:
    //   All fields are private;
    //   author and text are Strings, so are guaranteed immutable;
    //   timestamp is a mutable Date, so Tweet() constructor and getTimestamp()
    //        make defensive copies to avoid sharing the rep's Date object with clients.
```

## What an ADT specifiction may talk about
![[Pasted image 20260130154050.png]]

使用 ADT 的一大好处在于，我们可以用设计优良的 ADT 来代替复杂的前置和后置条件：
```ts
/**
 * @param set1 is a sorted set of characters with no repeats
 * @param set2 is likewise
 * @returns characters that appear in one set but not the other,
 *  in sorted order with no repeats
 */
static exclusiveOr(set1: string, set2: string): string;
```

可以封装为：
```ts
/**
 * @returns characters that appear in one set but not the other
 */
static exclusiveOr(set1: SortedCharSet, set2: SortedCharSet): SortedCharSet;
```

## How to establish invariants
在确立不变量时：
- Creator 和 Producer 需要**确立**新实例的不变量；
- Mutator、Observer 和 Producer 需要**保护**现有实例的不变量。
- 没有发生**表示泄露**。

以上三个性质，就可以在数学上充分保证不变量在任何情况下都满足。

## Recipes for programming
扩展 [[Testing 6102#Test-First Programming]] 的方法，编写一个 ADT 的三个步骤：
1. **规范**：为 ADT 的所有操作编写规范，包括方法签名、前置条件和后置条件。这个过程中需要定义**抽象值**。
2. **测试**：根据规范，为所有操作编写测试用例。此时**迭代**的思想依然生效，在编写测试用例时可能发现规范中的错误，进而进行修正。
3. **实现**：对于 ADT 而言，实现包括以下子步骤：
	1. 选取**表示**。写下类的**私有字段**，并在注释中描述**表示不变量**和**抽象函数**。
	2. **断言表示不变量**。编写 `checkRep()` 方法，强制操作检查表示不变量。
	3. **实现操作**。编写方法的函数体，确保测试全部通过。

进而，我们将编写函数和 ADT 的方法结合起来，形成**编写程序**的方法论：
1. 选取**ADT**：决定哪些数据是可以修改的，哪些是不可修改的。
2. 选取**函数**：编写顶层 `main` 函数，并将其分解为多个子步骤。
3. **规范**：为 ADT 和函数分别编写规范。开发初期应该让 ADT 保持简洁，尽量控制操作的总数。
4. **测试**：为每个**单元**（包括 ADT 和函数）编写单元测试。
5. **初步实现**：选择**最简单、暴力**的表示，尽早地将整个程序跑通，忽略高级功能/性能优化/边际情况等复杂任务。
6. **迭代**：拾起上一步中提及的复杂任务，一步步加以实现。

