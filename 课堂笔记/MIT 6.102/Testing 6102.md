#mit6102 
## Validation
程序**验证**，旨在通过发现程序中的问题，来使开发者和用户更加确信程序的正确性。验证包括以下方面：
- **形式推理**(Formal Reasoning)：也叫 Verification，通过形式化的推理来证明程序的正确性。这种方式最可靠，但难以人工完成，通常只用于体量较小、较为核心的模块。
- **代码审查**(Code Review)：让其他人阅读你的代码，并进行非形式化的推理，来发现漏洞。
- **测试**(Testing)：使用精心设计的输入数据运行程序，检查运行结果是否正确。

## Wrong Testing Approches
在软件工程领域，存在一些典型的错误测试方式，应当在实际开发中避免：
- **彻底测试**(Exhaustive Testing)：即测试所有的可能输入，然而这是不可能的，因为可能的输入非常多，不可能完全覆盖。
- **随意测试**(Haphazard Testing)：即写完程序之后随手跑一下，看看能不能运行。然而随意选择的输入通常无法覆盖大部分典型的输入情况，也就难以查明 Bug。
- **随机/统计测试**(Random/Statistical Testing)：即测试小规模的随机样本，计算产品缺陷率，通常用于实物产品领域。然而，在软件领域，程序的行为随输入的变化是离散的，尤其在某些边界情况可能突然出现问题。不过，随机测试仍然可以作为辅助，增强程序员对程序正确性的信心。

## Test-First Programming

^4256d7

**模块**(Module)：可以从整个软件系统分离出来，独立地进行设计、实现和测试的部分。

**规范**(Specification, 简称 Spec)：描述一个模块的行为的文档。对于函数来说，规范限定了函数参数的类型和额外约束，以及返回值的类型和其与各个参数的关系。

**实现**(Implementation)：使得模块表现出预期行为的具体步骤，例如函数的代码块。

**客户端**(Client)：使用模块的对象。

**测试用例**(Test Case)：某个特定的输入 + 根据规范预期得到的特定输出。

**测试套件**(Test Suite)：用于测试一个模块的测试用例的集合。

**测试先行编程**(Test-First Programming) 是一个重要的软件开发思想，要求在写模块实现之前，优先编写模块规范和测试套件，即按以下顺序实现模块：
1. 规范
2. 测试套件
3. 实现

## Systematic Testing
**系统测试**(Systematic Testing)指规范化地选取测试用例，从而构建一套具有以下优点的优秀测试套件：
- **正确性**(Correct)：测试套件是**合法的**客户端，且接受任何**合法的**对规范的实现。
- **充分性**(Thorough)：能尽可能发现具体实现中可能产生的 Bug。
- **小规模**(Small)：包含的测试用例尽可能少，可以被快速编写出来、容易更新迭代也能够快速运行。

## Partitioning
那么在系统测试中，应该如何选取测试用例呢？通常，我们应当将模块的输入空间**划分**(Partition)为多个子域，然后在每个子域中分别选取一个输入，来构建测试用例。

具体而言，**划分**就是将输入空间分割为多个**不相交的**(disjoint)、**完整的**(complete，即并集可以覆盖整个空间)、**非空的**(non-empty)子空间，再在每个子空间中取出一个输入作为代表，从而在测试资源有限的情况下**充分**地测试不同输入情况下的模块行为。

特别地，由于边界情况是 Bug 重灾区，应当独立为一个子空间处理，例如：
- 实数 0。
- 数值类型的最大/最小值，如 `Number.MAX_SAFE_INTEGER` or `Number.MAX_VALUE`。
- 空字符串/空列表/空集合……
- 序列的第一个/最后一个元素。
- etc.

## Mutlitple Partitioning
**多重划分**，是指针对不同的测试点（如不同的参数 a 与 b、参数 a 的范围与符号等），然后再挑选测试用例，覆盖所有划分的所有子空间。注意，同一个测试用例可以覆盖多个划分。

相比于单次划分，多重划分可以大大减少划分的子域数（例如单次划分需要 `a的情况数*b的情况数` 个子域，而对 a、b 分别划分只需要总计 `a的情况数+b的情况数` 个子域），同时一个测试用例可以覆盖多个子域，从而减少所需测试用例的数量。

然而，多重划分将不同的划分独立开来，从而可能忽略了它们之间的相互作用，因此有必要进行权衡。

## Documenting Testing Strategy
在测试代码记录测试策略是必要的，可以方便后续的阅读、调试和维护。

单划分策略：
```ts
describe("max", function() {
  /*
   * Testing strategy
   *
   * partition:
   *   a < b
   *   a > b
   *   a = b
   */

   it(...); // test cases here
   it(...);
   it(...);
   ...
});
```

多划分策略：
```ts
describe("multiplication", function() {
  /*
   * Testing strategy
   *
   * cover the cartesian product of these partitions:
   *   partition on a: positive, negative, 0
   *   partition on b: positive, negative, 0
   *   partition on a: 1, !=1
   *   partition on b: 1, !=1
   *   partition on a: small (fits in a TypeScript number), or large (doesn't fit)
   *   partition on b: small, large
   *
   * cover the subdomains of these partitions:
   *   partition on signs of a and b:
   *      both positive
   *      both negative
   *      different signs
   *      one or both are 0
   */

  it("covers a = 1, b != 1, a and b have same sign", function() {
    assert.strictEqual(BigInt(1) * BigInt(33), BigInt(33));
  });

  it("covers a is positive, b is negative, "
      + "a fits in a number, b fits in a number, "
      + "a and b have different signs", function() {
    assert.strictEqual(BigInt(73) * BigInt(-2), BigInt(-146));
  });

  ...
});
```

## Black box and Glass box Testing 
**黑箱测试** (Black Box Testing)：指只根据模块规范选取测试用例，而与模块的具体实现无关。

**白箱测试**(Glass Box Testing)：指根据具体实现选取测试用例，以针对性地测试该实现特有的边界情况。例如，如果某个实现使用了内置缓存，那么应该多次调用同一组测试用例，观察缓存是否正常工作。

**注意**：白箱测试依然要***保证正确性***，即仍接受其他不同实现，而非只有特定实现能够通过测试。白箱测试的目的是根据特定实现做针对性的划分，但依然忠实于规范。

## Coverage
**覆盖程度** (Coverage) 是一个评估测试的充分程度的好方法。有三种常见的覆盖范围：
- **语句覆盖** (Statement Coverage)：每个语句是否都被至少一个测试用例运行。
- **分支覆盖** (Branch Coverage)：每个分支语句的每个分支是否都被至少一个测试用例选择。
- **路径覆盖** (Path Coverage)：每个分支的组合情况是否都被至少一个测试用例遍历。

以上三种覆盖依次增强，需要的测试用例数依次增多。在工业界，**语句覆盖**是普遍预期达成的目标，**分支覆盖**的要求更高，而**路径覆盖**由于其所需测试用例随分支数量指数增长，通常被认为是不合适的。

一种规范的测试方式是：不断地增加测试，直到测试套件能够达成**语句覆盖**水平为止即可。使用代码覆盖检查工具（如使用 c8 测试 JS/TS 代码），可以完成模块实现后，在黑箱测试的基础上扩充得到 100% 覆盖的白箱测试。

## Unit and Integration Testing
以上我们所讨论的都属于**单元测试**的范畴，即独立地测试某个模块的行为是否符合预期。

而**集成测试**(Integration Testing) 会结合测试多个模块乃至整个程序。集成测试同样重要，因为一个程序通常涉及到多个模块的相互作用。

## Regression Testing
**回归测试**(Regression Testing) 是指在修复其他漏洞、增加新功能或进行代码优化后重新运行已测试代码的测试，来防止原来的代码因新的改动出现**退化**(Regress)。

此外，每次修复一个 Bug 后，引入导致该 Bug 的输入作为测试用例，这个用例就被称为**回归测试用例**。这也可以用于**测试先行调试**，即先将导致 Bug 的输入加入测试套件，再修改实现使之通过。

**回归测试**往往与**自动测试**相结合，统称为**自动回归测试**(Automated Regression Testing)。

## Iterative Test-First Programming
**迭代测试先行编程**(Iterative Test-First Programming) 是对测试先行编程的改进，它允许程序员在进行后续步骤的同时，发现问题并返回改进原先的步骤。

具体来说，迭代测试先行编程的三个步骤为：
1. 编写规范。
2. 为规范编写测试。如果发现问题，就为规范和测试进行迭代。
3. 根据规范和测试编写实现。如果发现问题，就为规范、测试和实现进行迭代。

编写测试可以更好地理解规范，发现规范中错误/不完整/模糊的部分；而编写实现则可以发现测试中缺失或错误的部分，进而完善前面的部分。

由于迭代的存在，一开始就没有必要对每个步骤都精益求精，例如：
- 对于规模较大的规范，可以先编写一部分的规范，完成该部分的测试和实现，最后再返回去继续迭代其他部分。
- 对于复杂的测试套件，可以先选取一些比较重要的划分，创建一个规模较小的测试套件，基于此测试完成一个简单的实现，再返回去继续迭代测试套件。
- 对于复杂的实现，可以先编写一个简单的暴力算法验证规范和测试的正确性，再迭代实现更加高效但复杂的算法。

**迭代**的核心思想在于，不要试图在一开始就给出完美的实现，而是优先尽快给出一个大致实现方案（Just Make It Work），再逐步打磨、优化，其高效性已经被 Agile、Scrum 等软件开发模式所证实。

## Randomized Testing
对于已经经过系统测试验证的软件，可以考虑使用随机测试作为辅助增强我们对软件的信心：
- **模糊测试**(Fuzz Testing)：为函数或整个程序生成随机的输入，观察程序是否会因此崩溃（而不关心具体的运行结果，因为我们其实并不知道结果）。
- **基于属性测试**(Property-Based Testing)：同样随机地生成输入，再检查结果是否满足特定（根据规范必然满足的）条件。例如：测试一个字符串切片函数，可以随机生成参数，检查返回值是否为原字符串的子串。基于属性测试是模糊测试的超集，模糊测试可以被视为测试“程序不会崩溃”这一属性。