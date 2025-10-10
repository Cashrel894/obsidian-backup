- Primitive Expressions: 2, 3.3, true, +, quotinent, ...
- Combinations: (quotinent 10 2), (not true), ...

调用表达式包含一个操作符和任意个数的操作数，并由小括号包裹。
 ```scheme
 > (+ 1 2)
 3
 > (+ 1 2 3 4)
 10
 > (+)
 0
 > (* 1 2 3 4)
 24
 > (*)
 1
 > (quotient 10 2) 
 5
 ```
 内置 Procedure：
 \+ - \* / and or not > < = >= <= quotient modulo ... j

```scheme
> +
#[+]
> (number? 3)
#t
> (zero? 0)
#t
> (integer? 2.2)
#f
```
