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
### Initialization
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

### Index
注意：`DataFrame` 中的 index 可以是不唯一的。此外，行的下标和行的 index 标签是不一样的。
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
### Column
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

## Slicing
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

注：使用 `[]` 选取列时，不支持 slice 操作。
```python
elections["Candidate"]
```
![[../../附件/Pasted image 20260101223223.png]]

```python
# Ask yourself: why is :9 is the correct slice to select the first 10 rows?
babynames_first_10_rows = babynames.loc[:9, :]

# Notice how we have exactly 10 elements in our boolean array argument
babynames_first_10_rows[[True, False, True, False, True, False, True, False, True, False]]
```

```python
# First, use a logical condition to generate a boolean array
logical_operator = (babynames["Sex"] == "F")

# Then, use this boolean array to filter the DataFrame
babynames[logical_operator].head()
```

![[../../附件/Pasted image 20260102095443.png]]

```python
names = ["Bella", "Alex", "Narges", "Lisa"]
babynames["Name"].isin(names).head()
babynames[babynames["Name"].isin(names)].head()

# Identify whether names begin with the letter "N"
babynames["Name"].str.startswith("N").head()
# Extracting names that begin with the letter "N"
babynames[babynames["Name"].str.startswith("N")].head()
```

### Column Modification
```python
# Create a Series of the length of each name. 
babyname_lengths = babynames["Name"].str.len()

# Add a column named "name_lengths" that includes the length of each name
babynames["name_lengths"] = babyname_lengths
babynames.head(5)
```

```python
# Rename “name_lengths” to “Length”
babynames = babynames.rename(columns={"name_lengths":"Length"})
babynames.head()
```

```python
# Drop our new "Length" column from the DataFrame
babynames = babynames.drop("Length", axis="columns") # 若不指定axis，则默认删除行
babynames.head(5)
```

### Useful Utility Functions
***几乎所有 Numpy 函数都可以作用于 `Series` 和 `DataFrame`***

```python
# Return the shape of the DataFrame, in the format (num_rows, num_columns)
babynames.shape

# Return the size of the DataFrame, equal to num_rows * num_columns
babynames.size
```

```python
babynames.describe()
```
![[../../附件/Pasted image 20260102220445.png]]

```python
# Sample a single row
babynames.sample()

# Sample 5 random rows, and select all columns after column 2
babynames.sample(5).iloc[:, 2:] # 默认不放回抽样

# Randomly sample 4 names from the year 2000, with replacement, and select all columns after column 2
babynames[babynames["Year"] == 2000].sample(4, replace = True).iloc[:, 2:]
```

```python
babynames["Name"].value_counts().head()
```
![[../../附件/Pasted image 20260102220621.png]]

```python
babynames["Name"].unique()
```
![[../../附件/Pasted image 20260102220641.png]]

```python
# Sort the "Name" Series alphabetically
babynames["Name"].sort_values(ascending=True).head()

babynames.sort_values("Name", key=lambda x: x.str.len(), ascending=False).head()
```

```python
# First, define a function to count the number of times "dr" or "ea" appear in each name
def dr_ea_count(string):
    return string.count('dr') + string.count('ea')

# Then, use `map` to apply `dr_ea_count` to each name in the "Name" column
babynames["dr_ea_count"] = babynames["Name"].map(dr_ea_count)

# Sort the DataFrame by the new "dr_ea_count" column so we can see our handiwork
babynames = babynames.sort_values(by="dr_ea_count", ascending=False)
babynames.head()
```


![[../../附件/Pasted image 20260102221657.png]]

## Groupby
```python
babynames[["Year", "Count"]].groupby("Year").agg("sum")
```
![[../../附件/Pasted image 20260102221006.png]]

![[../../附件/Pasted image 20260102221118.png]]

![[../../附件/Pasted image 20260102221252.png]]

![[../../附件/Pasted image 20260102221336.png]]

```python
grouped_by_party = elections.groupby("Party")
grouped_by_party.groups
```
```
{'American': [22, 126], 'American Independent': [115, 119, 124], 'Anti-Masonic': [6], 'Anti-Monopoly': [38], 'Citizens': [127], 'Communist': [89], 'Constitution': [160, 164, 172], 'Constitutional Union': [24], 'Democratic': [2, 4, 8, 10, 13, 14, 17, 20, 28, 29, 34, 37, 39, 45, 47, 52, 55, 57, 64, 70, 74, 77, 81, 83, 86, 91, 94, 97, 100, 105, 108, 111, 114, 116, 118, 123, 129, 134, 137, 140, 144, 151, 158, 162, 168, 176, 178, 183], 'Democratic-Republican': [0, 1], 'Dixiecrat': [103], 'Farmer–Labor': [78], 'Free Soil': [15, 18], 'Green': [149, 155, 156, 165, 170, 177, 181, 184], 'Greenback': [35], 'Independent': [121, 130, 143, 161, 167, 174, 185], 'Liberal Republican': [31], 'Libertarian': [125, 128, 132, 138, 139, 146, 153, 159, 163, 169, 175, 180], 'Libertarian Party': [186], 'National Democratic': [50], 'National Republican': [3, 5], 'National Union': [27], 'Natural Law': [148], 'New Alliance': [136], 'Northern Democratic': [26], 'Populist': [48, 61, 141], 'Progressive': [68, 82, 101, 107], 'Prohibition': [41, 44, 49, 51, 54, 59, 63, 67, 73, 75, 99], 'Reform': [150, 154], 'Republican': [21, 23, 30, 32, 33, 36, 40, 43, 46, 53, 56, 60, 65, 69, 72, 79, 80, 84, 87, 90, 96, 98, 104, 106, 109, 112, 113, 117, 120, 122, 131, 133, 135, 142, 145, 152, 157, 166, 171, 173, 179, 182], 'Socialist': [58, 62, 66, 71, 76, 85, 88, 92, 95, 102], 'Southern Democratic': [25], 'States' Rights': [110], 'Taxpayers': [147], 'Union': [93], 'Union Labor': [42], 'Whig': [7, 9, 11, 12, 16, 19]}
```

```python
grouped_by_party.get_group("Socialist")
```
![[../../附件/Pasted image 20260102221620.png]]

注意：`Groupby` 对象可以直接调用 `sum` `mean` `max` `min` `first` `last` `head(n)` `tail(n)` 等方法，与调用对应的 `agg` 方法行为一致。

 `size` 方法计算每组的**总行数**，并返回一个 `Series`。注意 `size` 方法和 `value_count` 方法本质上是一样的，只不过 `value_count` 方法会自动按照 count 降序排序。
```python
df.groupby("letter").size()
```
![[../../附件/Pasted image 20260102221847.png]]

而 `count` 方法则会分别返回每组**各列的非缺失值个数**，返回一个 `DataFrame`。
```python
df.groupby("letter").count()
```
![[../../附件/Pasted image 20260102221858.png]]

`groupby.filter` 接受一个函数，该函数应接受一个 `DataFrame`，返回一个 bool 值。接着，所有分组的子 `DataFrame` 作为实参传入该函数，并保留且仅保留返回值为 True 的分组，再按原行顺序放回返回一张 `DataFrame`。

```python
elections.groupby("Year").filter(lambda sf: sf["%"].max() < 45).head(9)
```

## Pivot
```python
# The `pivot_table` method is used to generate a Pandas pivot table
import numpy as np
babynames.pivot_table(
    index = "Year",
    columns = "Sex",    
    values = "Count", 
    aggfunc = "sum", 
).head(5)
```
![[../../附件/Pasted image 20260102222943.png]]

## Merge
```python
merged = pd.merge(left = elections, right = babynames_2022, \
                  left_on = "First Name", right_on = "Name")
merged.head()
# Notice that pandas automatically specifies `Year_x` and `Year_y` 
# when both merged DataFrames have the same column name to avoid confusion

# Second option
# merged = elections.merge(right = babynames_2022, \
    # left_on = "First Name", right_on = "Name")
```

