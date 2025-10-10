[Scheme Interpreter | CS 61A Fall 2024](https://insideempire.github.io/CS61A-Website-Archive/proj/scheme.html#extra-challenge)
## 动机
递归相比迭代而言，最大的区别在于递归会占用更大的空间复杂度。例如，遍历一个长度为 n 的列表，两者的时间复杂度都为 O(n)，但递归的空间复杂度为 O (n)，而迭代的复杂度为 O (1)。

这是因为，递归会在内存中保留运算的中间值，因此可以考虑消除这些中间值来实现优化。

## Tail Calls
![[../../附件/Pasted image 20250812211619.png]]
简单来说，如果递归函数在被调用后不需要完成额外的运算，那么这样的调用经尾调用优化后就可以跳过中间的调用过程、直接返回结果，从而节省内存。这样的调用称为尾调用。

通常线性的递归函数都可以写成使用尾调用的形式。

## 实现
Trampolining 蹦床函数优化
原理：将尾函数调用包装为一个未经计算的 thunk，然后只在需要的时候解包并计算它。

### Thunk
代表一个未经计算的函数，最常见的打包方式就是创建一个没有参数的 lambda 表达式，直接调用该 lambda 函数即可解包。
```python
>>> f = lambda: print('Hello World!')
>>> f()
Hello World!
```

thunk 可以相互嵌套，也可以用程序自动完成彻底的解包。这个解包过程称作 trampolining。
```python
def trampoline(val):
	while callable(val):
		val = val()
	return val
```

PS: 推测得名原因是，解包过程中环境如下所示变化，就像上下弹跳一样，始终只占用常数级别的空间。
![[../../附件/Pasted image 20250812220440.png]]

对应的代码：
![[../../附件/Pasted image 20250812220522.png]]

