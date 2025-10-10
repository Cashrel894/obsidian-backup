Scheme 中一个不是调用表达式的 Combination 就是 special form。

## Fundamentals
**If** expression: `(if <predicate> <consequent>[ jjj<alternative>])`
**And** and **Or**: `(and <e1> <e2> ... <en>)` `(or <e1> <e2> ... <en>)`
Binding symbols: `(define <symbol> <expression>)`
New procedures: `(define (<symbol> <formal parameters>) <body>)` 其中\<body>可以包含多个表达式，但只会返回最后一个。

```scheme
> (define (abs x)
	(if (< x 0)
		(- x)
		x))
> (abs -2)
2
```

## Function Defining
用 define 创建一个新 Procedure 时，步骤如下：
1. 用所给的参数和函数体创建一个 Procedure。
2. 将 Procedure 绑定到 symbol
3. 返回 symbol

## Lambda Expressions
Scheme 支持 lambda 表达式： `(lambda (<formal-parameters>) <body>)`
返回一个 Procedure
```scheme
(define (plus4 x) (+ x 4))
(define plus4 (lambda (x) (+ x 4))
```

## Cond & Begin
```scheme
(cond (<cond1> <exp1>)
	  (<cond2> <exp2>)
	  ...
	  (<condn> <expn>)
	  (else    <exp_else>)) ;类似if-elif-else结构

(begin <exp1> <exp2> ... <expn>) ;顺序执行表达式
```

## Let Expressions
赋予值一个临时变量名，只在一个表达式内使用。
```scheme
(let ((<name1> <exp1>) ... (<namen> <expn>)) <EXP>)

> (let ((a 3) (b 4)) (sqrt (+ (* a a) (* b b))))
25
```

