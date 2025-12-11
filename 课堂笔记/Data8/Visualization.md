#Data8 
## Scatter Plots
```python
actors.scatter('Number of Movies', 'Total Gross')
```
![[../../附件/Pasted image 20251209152503.png]]

## Line Plots
```python
movies_by_year.plot('Year', 'Number of Movies')
```
![[../../附件/Pasted image 20251209153359.png]]
## Bar Chart
```python
icecream.barh('Flavor', 'Number of Cartons') # barh = bar_horizontal
```
![[../../附件/Pasted image 20251209153334.png]]

## Histogram
不同数值 bin 的概率分布可视化。
```python
millions.hist('Adjusted Gross', unit="Million Dollars")
```
![[../../附件/Pasted image 20251211082857.png]]

### Density
```python
millions.hist('Adjusted Gross', bins=np.arange(300,2001,100), unit="Million Dollars")
```
注意：y 轴指的是 **bin 中元素对应的百分比（以%为单位） / bin 的宽度**，即***概率密度***。也就是说，每个长方形的**面积**才是 bin 的百分比，从而遵循了***面积原理***。

例如上图中：
![[../../附件/Pasted image 20251211091328.png]]

由此可知，所有长方体的面积之和为 **100%**。

之所以用面积表示概率，是因为直方图后续可以被拟合为一条平滑的 **概率密度曲线**，其在一个区间上的定积分就是这个区间所对应的概率。

```python
uneven = make_array(300, 350, 400, 500, 1800)
millions.hist('Adjusted Gross', bins=uneven, unit="Million Dollars")
```
![[../../附件/Pasted image 20251211091859.png]]
有时，bin 的宽度不一定均匀，此时面积准则依旧生效。

此时，如果用概率（或数量）代替概率密度，就会导致可视化严重失真：
```python
millions.hist('Adjusted Gross', bins=uneven, normed=False)
```
![[../../附件/Pasted image 20251211092117.png]]



## Overlaid Graphs
```python
name_of_table.method(column_label_of_common_axis, array_of_labels_of_variables_to_plot)
# name_of_table.method(充作x轴的列，充作y轴的列的列表)
name_of_table.method(column_label_of_common_axis)
# 默认以其他所有列为y轴
```
![[../../附件/Pasted image 20251210202659.png]]
![[../../附件/Pasted image 20251210215541.png]]
![[../../附件/Pasted image 20251210215613.png]]