```scheme
(append l1 l2 ... ln) ;将li的元素组成一个新列表
(map f s) ;将s的每个元素用f映射后组成一个新列表
(filter f s) ;返回包含s中f映射结果为#t的元素的列表
(apply f s) ;将s中的所有元素作为参数调用f

(define (range s t) (
        if (>= s t)
        nil
        (cons s (range (+ s 1) t))
     ))
```