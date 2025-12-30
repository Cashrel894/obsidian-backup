#DS100 
`Series`：***1 维带标签列表***，可以理解为**列数据**。
`Dataframe`：***2 维表格***。
`Index`：***由行/列标签组成的序列***。

## Series
```python
s = pd.series(["welcome", "to", "data 100"])
s
```
![[../../附件/Pasted image 20251230204645.png]]

```python
s.values
```
```
array(['welcome', 'to', 'data 100'], dtype=object)
```

```python
s.index
```
```
RangeIndex(start=0, stop=3, step=1)
```

```python
s = pd.Series([-1, 10, 2], index = ["a", "b", "c"])
s
```
![[../../附件/Pasted image 20251230204811.png]]

```python
s.index
```
```
Index(['first', 'second', 'third'], dtype='object')
```

## Selection in Series
```python
s["a"]
s[["a", "c"]]
```

```python
s > 0
```
![[../../附件/Pasted image 20251230205045.png]]

```python
s[s > 0]
```

