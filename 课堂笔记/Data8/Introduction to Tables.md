#Data8 
**表格**是表示数据集的基本方式之一。一张表格可以用两种视角看待：
- 一系列有名称的***列***，每列都描述各个对象的某个单一属性。
- 一系列***行***，每行都描述某一对象的所有属性。

 `datascience` 库提供了一些操作表格的方法。
```python
table.show(2) # 显示表格的前两行
table.select('col1', 'col2') # 返回一张只包含指定列的新表格
table.drop('col') # 返回一张丢弃指定列的新表格
table.sort('col') # 返回一张根据指定列升序排序的新表格
table.sort('col', descending=True) # 降序排序
table.num_columns # 表格的列数
table.num_rows # 表格的行数
```

```python
table.where('col', 'content') # 返回一张新表格，只包含指定列为指定内容的行
table.where('col', are.equal_to('content')) # 或者接受一个谓词，描述选中的行所需要满足的性质
# 一些其他谓词：
are.equal_to
are.not_equal_to
are.above
are.above_or_equal_to
are.below
are.between
are.between_or_equal_to
```