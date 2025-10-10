## quote
用'a 表示符号 a，而不是 a 所代表的值。'a 是 (quote a) 的缩写形式。

而 Scheme 中，表达式的本质就是一系列符号组成的列表，进行运算处理。

```scheme
> (list 'define 'list)
(define list)
> (car '(a b c))
a
> (cdr '(a (1 2) c))
((1 2) c)
```

## quastiquote
用反引号\`同样可以将表达式转换为符号，而与'的区别在于，用反引号转换的表达式可以用, 转回表达式。
```scheme
> '(a ,(+ b 1))
(a (unquote (+ b 1)))
> `(a ,(+ b 1))
(a 5)
```
使用反引号，可以方便地生成 Scheme 表达式。
```scheme
> (define (make-add-procedure n) `(lambda (d) (+ d ,n)))
make-add-procedure
> (make-add-procedure 2)
(lambda (d) (+ d 2))
```