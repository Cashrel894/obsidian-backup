## 遍历字典
```python
>>> numerals = {'a': 1, 'b': 2, 'c': 3}
>>> list(numerals)
['a', 'b', 'c']
>>> list(numerals.values())
[1, 2, 3]
```
因此，可以用 for 循环遍历字典的键或值。

## 键的限制
- 不能重复。
- 不能是列表或字典

## 字典解析
```python
{<key exp>: <value exp> for <name> in <iter exp>( if <filter exp>)}
```