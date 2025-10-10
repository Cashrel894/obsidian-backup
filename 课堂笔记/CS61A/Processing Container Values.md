## sum
```python
sum(iterable[, start])
# 返回一个由数字组成的iterable中所有元素的总和（+start，start默认为0）。
```
注：不适用于 string，但适用于列表，例：
```python
>>> sum([[1, 2], [3]], [])
[1, 2, 3]
>>> sum([[1, 2], [3]])
Error
```

## max
```python
max(iterable[, key=func])
max(a, b, c, ...[, key=func])
# 返回iterable/所有参数中的最大值。
# 如果提供key函数（单参数），那么返回key运算结果最大的元素。
```

## all
```python
all(iterable)
# 若iterable中所有元素都为真值，则返回True，否则返回False。
```