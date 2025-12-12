#Data8 
```python
more_cones.group(['Flavor', 'Color']) # 遍历两列的所有可能组合，进行grouping
```
![[../../附件/Pasted image 20251212195854.png]]

```python
more_cones.pivot('Flavor', 'Color')
```
![[../../附件/Pasted image 20251212195920.png]]
`pivot` 方法和 `group` 很类似，它可以将多个某些列值组合相同的行合并为单一数值。只不过，`pivot` 是以网格形式呈现的。

```python
more_cones.pivot('Flavor', 'Color', values='Price', collect=sum)
```
![[../../附件/Pasted image 20251212200306.png]]