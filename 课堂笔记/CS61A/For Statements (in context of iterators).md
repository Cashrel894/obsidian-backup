```python
for i in iterator:
	do_something()
```
for 循环也可以用于迭代器的迭代，iterator 会从当前状态起向后迭代到终点，并将每次迭代的值绑定到 i 。

```python
>>> l = [1, 2, 3]
>>> t = iter(l)
>>> next(t)
1
>>> for i in t:
...		print(i)
2
3
```