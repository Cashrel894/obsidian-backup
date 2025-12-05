#Kaggle 

```python
reviews.country
reviews['country']
reviews['country'][0]
```
## loc & iloc
`iloc` 基于**下标**来选取数据，先指定行号，再指定列号。
```python
reviews.iloc[0] # 选中第一行
reviews.iloc[:, 0] # 选中第一列
reviews.iloc[:3, 0]
reviews.iloc[1:3, 0]
reviews.iloc[[0, 1, 2], 0]
reviews.iloc[-5:]
```

`loc` 则基于**标签**，同样先行后列：
```python
reviews.loc[0, 'country']
reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
```

***注意***：切片在 `iloc` 与原版一致，为左闭右开；而在 `loc` 中为闭区间。

## Manipulating the index
`set_index` 可以将某个特定的列设置为索引，这样就可以直接用 `loc` 进行访问。
```python
reviews.set_index("title")
```

## Conditional selection
```python
reviews.loc[reviews.country == 'Italy']
reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
reviews.loc[reviews.country.isin(['Italy', 'France'])]
reviews.loc[reviews.price.isnull()]
reviews.loc[reviews.price.notnull()]
```

## Assigning data
```python
reviews['critic'] = 'everyone' # 将critic列的所有元素都设为'everyone'
reviews['index_backwards'] = range(len(reviews), 0, -1)
```