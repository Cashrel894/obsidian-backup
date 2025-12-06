#Kaggle 
## Summary Functions
```python
reviews.points.describe() # 显示指定列的一些统计信息，如count、mean、min、max等。
```
![[../../附件/Pasted image 20251206142506.png]]
对于非数字类型的数据，有不同的统计数据：
![[../../附件/Pasted image 20251206142526.png]]
```python
reviews.points.mean()
reviews.taster_name.unique()
reviews.taster_name.value_counts()
```

## Maps
```python
review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
```

```python
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```
