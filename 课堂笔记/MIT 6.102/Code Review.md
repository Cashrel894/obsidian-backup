#mit6102 
**代码审查**(Code Review)，是指细致、系统地研究其他人所编写的源代码，可以类比为 Proofread 他人的论文。

代码审查有两大目的：
- **提高代码质量**。他人的审查可以帮忙找到 Bug、发现可能的漏洞、检查代码的清晰程度、检查代码风格的一致性。
- **提高开发能力**。代码审查本身也是程序员之间教学相长的过程，内容可能包括新的语言特性、项目结构或代码规范的变化、新的编程技巧等。

## Style Standards
**代码风格规范**(Coding Style Standards) 是对开发团队代码风格的指导标准，用于提高项目代码的一致性与可读性，从而增强开发效率。

具体的风格规范往往非常细致入微，且因项目而异。然而，也存在一些通用的开发规范，值得每位开发者学习。

### Don't Repeat Yourself (DRY)
程序员应当尽可能避免代码的重复，因为若重复的代码内部存在 Bug，程序员需要修改多处代码才能根除漏洞，而这就埋下了安全隐患。

### Comment Where Needed
注释的重要性不言自明。几类有必要包含的注释有：
- 模块规范 (Specification)。
- 代码来源/出处。从其他地方 copy 的代码有时会牵扯到著作权问题，如各种要求表明作者的开源协议；同时 copy 的代码也有可能有漏洞/过时，记录出处可以帮助我们追根溯源、找出 Bug 源头。
- 代码本身的运作原理/存在原因不甚清晰，需要额外的文字说明。

### Fail Fast
**Failing Fast** 是一种重要的开发原则，强调代码应当尽可能早地暴露出可能的 Bug，以便后续修复，早发现早治疗。例如，静态检查早于动态检查，动态检查早于 Wrong Answer。

### Avoid Magic Numbers
**魔数**(Magic Number) 是指内联于代码中，出现的原因或原理不明的常值。

当我们需要使用到常量时，应当尽可能避免魔数的出现。我们可以为常数增加注释，但更好的办法是将常数赋值给一个 const 变量，并给一个易于理解的变量名，方便后续的阅读、调试和迭代。

### One Purpose For Each Variable
一个变量应当只承担一个任务。除非必要，一个变量应该尽可能避免重赋值，要尽可能多得使用 const 变量。例如，函数的参数就不应该被重赋值，以防后续迭代中需要访问参数的初始值。

### Use Good Names
https://medium.com/@rabinovichsagi/effectively-naming-software-thingies-fcea9d78a699

好的变量名通常是长而自描述的 (Self-descriptive)。好的变量名往往比注释更加重要。

### Use Whitespace and Punctuation
### Don't Use Global Variables
### Functions Should Return Results, Not Print Them
### Avoid Special-Case Code
程序员应当尽可能避免处理特殊情况。例如，在编写一个函数时，应当先编写通用代码，再考虑通用代码无法覆盖的部分解决特殊情况，否则可能导致代码的冗余。

### Code At The Right Length
单个函数不应该超过一页，单行代码不应该超过 80 个字符。

## Refactoring
**重构**(Refactor) 是指在不改变代码行为的前提下，改善代码以期提高软件的 SFB/ETU/RFC。

重构可大可小，大规模的重构则可能引入新的问题。安全的重构应当注意以下方面：
- Make small steps.
- Comment out old code temporarily.
- Use static checking to guide your changes.
- Run tests after every change.
- Delete old code and commit to version control after every smal steps.
