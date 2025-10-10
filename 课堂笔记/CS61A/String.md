## exec
```python
exec(<string>)
# 将string视为python语句并尝试解析和执行。
```

## String 字面量的多种形式
```python
'Ciallo~'
"I've done today's homework."
"""First Line,
Second Line,
Fourth Line,
Any Problem?"""
```

## String 也是一种序列
String 和列表有相似处：
```python
>>> greeting = "Ciallo~"
>>> len(greeting)
7
>>> greeting[2]
'a' # 注意返回的依然是一个String
```
不过 (not) in 会匹配子串：
```python
>>> 'all' in 'Ciallo~'
True
```