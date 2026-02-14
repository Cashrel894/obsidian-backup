#mit6102 
## Grammars
**语法**(Grammar)：用于描述和理解一串特定符号的**标准**，这里的符号串可以指：
- 字符串
- 文件
- 通过网络传输的信息
- 用户在命令行输入的指令
- ……

语法定义了一个字符串集合，其中有两类主要元素：**终止符**(Terminal) 和**非终止符**(Nonterminal)。
- 语法中的字符串字面量被称为**终止符**，因为它们无法被进一步拓展，例如 `"http"`、`":"`
- 而**非终止符**则指的是可以匹配多种可能字符串的变量，可以被进一步拓展。

为了定义非终止符可以匹配的值，语法还需要定义 **产生式** (Production)（或规则, rule）的集合。

产生式利用其他非终止符、操作符和终止符组成的表达式来定义一个非终止符：
```parserlib
nonterminal ::= expression of terminals, nonterminals, and operators 
```

一个语法会指定一个非终止符作为它的**根节点**(Root)，通常记作 `root` 或 `start` 或 `S`。

例如，一个只表示字符串"123456"的语法，只包含一个产生式：
```parserlib
num ::= "123456"
```
### Grammar Operators
产生式可以通过**运算符**(Operator) 来结合终止符和非终止符，三个最重要的操作符有：
- **Repetition**：`x ::= y*` 表示 x 可以匹配 0 或多个 y
- **Concatenation**：`x ::= y z` 表示 x 可以匹配 y 后接 z
- **Union**：`x ::= y | z` 表示 x 可以匹配 y 或 z

三个操作符的优先级依次降低。同时，可以使用括号来改变优先级：
```parserlib
m ::= a (b|c) d      // m matches a, followed by either b or c, followed by d
x ::= (y z | a b)*   // x matches zero or more yz or ab pairs
```

例：
```parserlib
url ::= 'http://' hostname '/'
hostname ::= word '.' word
word ::= letter*
letter ::= ('a' | 'b' | 'c' | 'd' | 'e' | 'f' | 'g' | 'h' | 'i' 
                | 'j' | 'k' | 'l' | 'm' | 'n' | 'o' | 'p' | 'q' 
                | 'r' | 's' | 't' | 'u' | 'v' | 'w' | 'x' | 'y' | 'z')
```

除此之外，还有一些语法糖运算符，可以被以上三种运算符表达出来，但可以方便我们使用：
- **0 or 1 occurrence**：`x ::= y?` 表示 x 为 y 或空字符串。
- **1 or more occurrences**：`x ::= y+` 表示 x 为 1 个或多个 y。
- **exact number of occurrences**：
```parserlib
x ::= y{3}     // an x is three y
               //    equivalent to x ::= y y y 

x ::= y{1,3}   // an x is between one and three y
               //    equivalent to x ::= y | y y | y y y

x ::= y{,4}    // an x is at most four y
               //    equivalent to x ::=   | y | y y | y y y | y y y y
               //                        ^--- note the empty string here

x ::= y{2,}    // an x is two or more y
               //    equivalent to x ::= y y y*
```
- **character class**：
```parserlib
x ::= [aeiou]     // equivalent to  x ::= 'a' | 'e' | 'i' | 'o' | 'u'
x ::= [a-ckx-z]   // equivalent to  x ::= 'a' | 'b' | 'c' | 'k' | 'x' | 'y' | 'z'
x ::= [^a-c]  // equivalent to  x ::= 'd' | 'e' | 'f' | ... | '0' | '1' | ... | '!' | '@'
              //                          | ... (all other possible characters)
```

### Recursion in Grammars
此外，产生式还可以递归定义：
```parserlib
url ::= 'http://' hostname (':' port)? '/' 
hostname ::= word '.' hostname | word '.' word
port ::= [0-9]+
word ::= [a-z]+
```

## Parse trees
使用语法匹配字符串，可以生成一棵**解析树**(Parse Tree)，来展示字符串的各个部分与语法的对应关系。

解析树的叶子节点都是终止符，中间结点则为非终止符。每个非终止符的子结点总是符合对应的产生式。
![[Pasted image 20260214192955.png]]

![[Pasted image 20260214193505.png]]

## Regular Expressions
如果在一个语法中，其根节点的产生式右侧的非终止符可以被完全替换为终止符和操作符的组合，则称这个语法是**正则**(Regular)的。

例：
```parserlib
url ::= 'http://' ([a-z]+ '.')+ [a-z]+ (':' [0-9]+)? '/' 
```

```parserlib
markdown ::= ([^_]* | '_' [^_]* '_' )*
```

而 HTML 则不是正则的，因为其中的递归产生式无法被完全替换：
```parserlib
html ::= ( [^<>]* | '<i>' html '</i>' )*
```

如果我们省略终止符周围的引号，以及连接运算符的空格，得到的产生式的右侧又可以被称为**正则表达式**(Regular Expression, Regex)。

正则表达式是表示正则语法的一种更紧凑、但又更难以阅读和理解的方式。