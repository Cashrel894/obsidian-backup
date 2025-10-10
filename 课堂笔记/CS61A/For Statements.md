## for 循环执行过程
```python
for <name> in <expression>:
	<suite>
```
1. 计算expression的值，结果必须是可迭代的（一个 iterable 序列）。
2. 对于序列中的每个项：
	1. 将 name 绑定到那个项。
	2. 执行 suite

## 序列解构
如果遍历的序列只由相同长度的序列组成，那么 for 循环中允许多个 name 出现，分别绑定到子序列的各个项。
```python
same_count = 0
for x, y in [[1, 2], [2, 2], [3, 2]]:
	if x == y:
		same_count += 1
print(same_count)
```

