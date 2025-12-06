#Kaggle 
## Group
```python
reviews.points.value_counts()
reviews.groupby('points').points.count() # 二者等价

reviews.groupby('points').price.min()
reviews.groupby('winery').apply(lambda df: df.title.iloc[0])
```

```python
reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])
```
![[../../附件/Pasted image 20251206155238.png]]

```python
reviews.groupby(['country']).price.agg([len, min, max])
```
![[../../附件/Pasted image 20251206155133.png]]

## Sort
```python
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
countries_reviewed.sort_values(by='len', ascending=False)
countries_reviewed.sort_index() # 按下标排序
countries_reviewed.sort_values(by=['country', 'len']) # 多关键字排序

```