列表是 mutable 的，因为列表中的数据可以被更改。

列表操作一览：
```python
>>> l = [1, 2, 3]
>>> l2 = l
>>> l.pop() # l.pop([index=-1]),移除index对应的元素
3
>>> l.remove(2) #移除第一个与参数相等的元素
>>> l
[1]
>>> l.append(4)
>>> l.extend([5, 6])
>>> l
[1, 4, 5, 6]
>>> l.insert(3, 0)
>>> l
[1, 4, 5, 0, 6]
>>> l[2] = 7
>>> l[0:2] = [8, 9, 10]
>>> l
[8, 9, 10, 6]
>>> l2
[8, 9, 10, 6]
```

部分字典操作：
```python
>>> dic = {'a': 1, 'b': 2, 'c': 3}
>>> dic.pop('b')
2
>>> dic.get('b') # return None
>>> dic.get('b', 'nothing') # get default value 'nothing'
'nothing'
>>> dic
{'a': 1, 'c': 3}
```