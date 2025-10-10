用 `iter(container)` 创建指定容器的迭代器，用 `next(iterator)` 获取迭代器指向的下一个元素。
```python
>>> s = [1, 2, 3, 4, 5]
>>> t = iter(s)
>>> next(t)
1
>>> next(t)
2
>>> list(t)
[3, 4, 5]
>>> next(t)
Error: StopIteration
```

## Iterable 和 Iterator
任何可以被传入 iter 函数的值都是 iterable 的。

iterator 被 iter 返回，可以作为参数传入 next。所有 iterator 都是 mutable 的。

## Dictionary Iteration
对于 Python 3.6+，字典中的元素按照键值对插入的顺序排序。

对于字典 d，将 d.keys()、d.values()、d.items() 分别传入 iter 可以得到字典的键、值、键值对的迭代器。如果直接传入 d，那么作用与 d.keys() 相同。
```python
>>> d = {"one": 1, "two": 2}
>>> t = iter(d.items())
>>> next(t)
("one", 1)
```

若迭代器创建后，原容器的 size 发生改变，那么再次对其调用 next 将报错。
```python
>>> t2 = iter(d)
>>> next(t2)
"one"
>>> d["zero"] = 0
>>> next(t2)
Error
```

## 使用 Iterator 的原因
- 泛用性强，对于 list tuple dict_keys 等各种可迭代对象都适用。