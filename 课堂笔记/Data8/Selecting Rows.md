#Data8 
```python
nba.take(0) # 取出第一行，返回table
nba.take(np.arange(3, 6)) # 取出3~5行
nba.sort('SALARY', descending=True).take(np.arange(5))
```

| **Predicate**                   | Description                                                 |
| ------------------------------- | ----------------------------------------------------------- |
| `are.equal_to(Z)`               | Equal to `Z`                                                |
| `are.above(x)`                  | Greater than `x`                                            |
| `are.above_or_equal_to(x)`      | Greater than or equal to `x`                                |
| `are.below(x)`                  | Less than `x`                                               |
| `are.below_or_equal_to(x)`      | Less than or equal to `x`                                   |
| `are.between(x, y)`             | Greater than or equal to `x`, and less than `y`             |
| `are.strictly_between(x, y)`    | Greater than `x` and less than `y`                          |
| `are.between_or_equal_to(x, y)` | Greater than or equal to `x`, and less than or equal to `y` |
| `are.containing(S)`             | Contains the string `S`                                     |

| **Predicate**         | Description      |
| --------------------- | ---------------- |
| `are.not_equal_to(Z)` | Not equal to `Z` |
| `are.not_above(x)`    | Not above `x`    |
以此类推，前述所有谓词都可以加上 `not_` 前缀。