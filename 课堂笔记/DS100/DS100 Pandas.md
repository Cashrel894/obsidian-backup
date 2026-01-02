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

## DataFrame
### initialization
```python
import pandas as pd
elections = pd.read_csv("data/elections.csv")
elections
```

```python
df_list = pd.DataFrame([1, 2, 3], columns=["Numbers"])
df_list
```
![[../../附件/Pasted image 20260101221539.png]]

```python
df_list = pd.DataFrame([[1, "one"], [2, "two"]], columns = ["Number", "Description"])
df_list
```
![[../../附件/Pasted image 20260101221559.png]]

```python
df_dict = pd.DataFrame({
    "Fruit": ["Strawberry", "Orange"], 
    "Price": [5.49, 3.99]
})
df_dict
```
![[../../附件/Pasted image 20260101221630.png]]

```python
df_dict = pd.DataFrame(
    [
        {"Fruit":"Strawberry", "Price":5.49}, 
        {"Fruit": "Orange", "Price":3.99}
    ]
)
df_dict
```
![[../../附件/Pasted image 20260101221648.png]]

```python
# Notice how our indices, or row labels, are the same

s_a = pd.Series(["a1", "a2", "a3"], index = ["r1", "r2", "r3"])
s_b = pd.Series(["b1", "b2", "b3"], index = ["r1", "r2", "r3"])

pd.DataFrame(s_a)
s_b.to_frame()
pd.DataFrame({
    "A-column": s_a, 
    "B-column": s_b
})
```

### index
注意：`DataFrame` 中的 index 可以是不唯一的。
```python
# Creating a DataFrame from a CSV file and specifying the index column
elections = pd.read_csv("data/elections.csv", index_col = "Candidate")
elections
```

```python
elections.reset_index(inplace = True) # Resetting the index so we can set it again
# This sets the index to the "Party" column
elections.set_index("Party")
```

```python
# This resets the index to be the default list of integer
elections.reset_index(inplace=True) 
elections.index
```
### column
`DataFrame` 中的 column 名必须唯一。
```python
elections.columns
```
```
Index(['index', 'Candidate', 'Year', 'Popular vote', 'Result', '%'], dtype='object')
```

```python
elections.shape
```
```
(187, 6)
```

## slicing
```python
# Extract the first 5 rows of the DataFrame
elections.head(5)

# Extract the last 5 rows of the DataFrame
elections.tail(5)

elections.loc[0, 'Candidate']
# 'Andrew Jackson'

elections.loc[[87, 25, 179], "Popular vote"]
# 87     15761254
# 25       848019
# 179    74216154
# Name: Popular vote, dtype: int64
```

`df.loc` 支持切片，但不同于 Python 自带的 slice，loc 的切片为闭区间。
```python
elections.loc[0:3, 'Year':'Popular vote']
```
![[../../附件/Pasted image 20260101222716.png]]

`iloc` 则使用编号而非标签选取元素，注意使用切片时，和 Python 自带的 slice 一样是左闭右开的。
```python
# elections.loc[0, "Candidate"] - Previous approach
elections.iloc[0, 1]

elections.iloc[[1,2,3],1]

# elections.loc[0:3, 'Year':'Popular vote'] - Previous approach
elections.iloc[0:4, 0:4]

#elections.loc[[0, 1, 2, 3], ['Year', 'Candidate', 'Party', 'Popular vote']] - Previous Approach
elections.iloc[[0, 1, 2, 3], [0, 1, 2, 3]]
```

```python
elections[0:4]
```
![[../../附件/Pasted image 20260101223149.png]]

```python
elections[["Year", "Candidate", "Party", "Popular vote"]]
```
![[../../附件/Pasted image 20260101223208.png]]

```python
elections["Candidate"]
```
![[../../附件/Pasted image 20260101223223.png]]

