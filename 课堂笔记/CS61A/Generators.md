生成器是一个迭代器，通过调用生成器函数产生，可以迭代产生生成器函数的 yield。

```python
def fib_generator():
	a, b = 0, 1
	while True:
		yield a
		a, b = b, a + b
```

```python
>>> fib = fib_generator()
>>> next(fib)
0
>>> next(fib)
1
>>> list([next(fib) for i in range(5)])
[1, 2, 3, 5, 8]
```

yield from 可以 yield 所有来自一个 iterator 或 iterable 的值。