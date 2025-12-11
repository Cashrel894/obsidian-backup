#Data8 
将不同的值放入不同的区间，将同一区间内的所有值整合为一个组。

类似于 `group`，但 `bin` 主要用于数值变量，而 `group` 用于分类变量。

```python
adj_gross = millions.column('Adjusted Gross')
min(adj_gross), max(adj_gross)
```
`(338.41, 1796.18)`

```python
bin_counts = millions.bin('Adjusted Gross', bins=np.arange(300,2001,100))
bin_counts.show()
```
![[../../附件/Pasted image 20251211081651.png]]
所有 bin 包含的区间都是***左闭右开***的。bin 中的所有值都对应其区间的左端点。特别地最后一个 bin 是**闭区间**，最后一行代表的是最后一个 bin 的**右端点**，其 **count 值永远为 0**。

```python
millions.bin('Adjusted Gross', bins=4)
```
![[../../附件/Pasted image 20251211082630.png]]

