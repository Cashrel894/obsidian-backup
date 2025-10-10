## Pairs
```scheme
(cons x y) ;返回Pair(x, y)
(car c) ;返回c的第一个元素
(cdr c) ;返回c的第二个元素
```

## List
本质是用 Pair 组建的链表。
```scheme
(list a1 a2 ... an)
nil '() ;均代表空列表
(list? l)
(null? l) ;判断列表l是否为空
```