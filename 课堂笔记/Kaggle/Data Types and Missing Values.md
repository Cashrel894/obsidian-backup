#Kaggle 
![[../../附件/Pasted image 20251206164124.png]]
![[../../附件/Pasted image 20251206164135.png]]
```python
reviews.points.astype('float64')
```

## Missing data
```python
reviews[pd.isnull(reviews.country)]

reviews.region_2.fillna("Unknown")

reviews.taster_twitter_handle.replace("@kerinokeefe", "@kerino")
```