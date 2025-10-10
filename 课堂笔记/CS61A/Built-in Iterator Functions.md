```python
map(func, iterable)
filter(func, iterable)
zip(first_iter, second_iter)
reversed(sequence)

list(iterable)
tuple(iterable)
sorted(iterable)
```

例：
```python
def palindrome(s):
	"""Return whether sequence s is the same backward and forward"""
	# My Solution
	# return all(map(lambda tp: tp[0] == tp[1], zip(s, reversed(s))))
	
	# Standard Solution
	return all([a == b for a, b in zip(s, reversed(s))]) # woc好优雅
```