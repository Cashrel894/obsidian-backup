柯里化是指将一个多参数函数转变为可以返回一个能接受其他参数的函数的单参数函数的过程。

例：
```python
def make_adder(a):
	return lambda b: a + b

>>> make_adder(2)(3)
5
>>> add(2, 3)
5
```
一般过程：
```python
# f(x) is a given double-argument function
def curry2(f):
	return lambda a: lambda b: f(a, b)
```