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

## Overlaid Graphs
```python
name_of_table.method(column_label_of_common_axis, array_of_labels_of_variables_to_plot)
# name_of_table.method(充作x轴的列，充作y轴的列的列表)
name_of_table.method(column_label_of_common_axis)
# 默认以其他所有列为y轴
```
![[../../附件/Pasted image 20251210202659.png]]