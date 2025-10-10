```python
def count(f):
	"""这个修饰器可以追踪函数被调用的次数，并存储在函数的call_count属性中。
	"""
	def counted(*args, **kw):
		counted.call_count += 1
		return f(*args, **kw)
	counted.call_count = 0
	return counted
```