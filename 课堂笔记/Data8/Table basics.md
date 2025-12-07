#Data8 
## Initializing tables
```python
Table() # 创建空表
Table().with_columns('Number of petals', make_array(8, 34, 5))
Table().with_columns(
    'Number of petals', make_array(8, 34, 5),
    'Name', make_array('lotus', 'sunflower', 'rose')
)

flowers = Table().with_columns(
    'Number of petals', make_array(8, 34, 5),
    'Name', make_array('lotus', 'sunflower', 'rose')
)
flowers.with_columns(
    'Color', make_array('pink', 'yellow', 'red')
) # 返回追加Color列的新表（对原表无影响!!!）

minard = Table.read_table(path_data + 'minard.csv')
```

## The size of the table
```python
minard.num_columns
minard.num_rows
```

## Column labels
```python
minard.labels
minard.relabeled('City', 'City Name')
```

## Accessing Data
```python
minard.column('Survivors') # 返回一个Array
minard.column(4)
minard.column(4).item(0)
minard.column(4).item(5)

minard.select('Longitude', 'Latitude') # 返回含有指定列的Table
minard.select(0, 1)
minard.drop('Longitude', 'Latitude', 'Direction')
```

## Formatting Data
```python
minard.set_format('Percent Surviving', PercentFormatter)
```
其他 formatter：DateFormatter，CurrencyFormatter 等。
