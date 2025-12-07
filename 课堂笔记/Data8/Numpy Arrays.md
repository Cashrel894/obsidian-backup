#Data8 
在 Numpy 中，一个 Array 只能存储一种类型的数据。
```python
english_parts_of_speech = make_array("noun", "pronoun", "verb", "adverb", "adjective", "conjunction", "preposition", "interjection")
```

Array 可以结合使用各种运算符，对 Array 中的元素逐个进行操作：
```python
(9/5) * highs + 32
```

Array 本身有一些实用的统计属性和方法：
```python
highs.size
highs.sum()
highs.mean()
```

Numpy 包提供了更多实用函数：
```python
np.diff(highs) # 创建目标Array的差分数组。新Array的第一个元素为原Array中第二个减去第一个，以此类推。 e.g.: [2, 3, 1, 4] => [1, -2, 3]
```

| **Function**       | Description                                                           |
| ------------------ | --------------------------------------------------------------------- |
| `np.prod`          | Multiply all elements together                                        |
| `np.sum`           | Add all elements together                                             |
| `np.all`           | Test whether all elements are true values (non-zero numbers are true) |
| `np.any`           | Test whether any elements are true values (non-zero numbers are true) |
| `np.count_nonzero` | Count the number of non-zero elements                                 |

| **Function** | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| `np.diff`    | Difference between adjacent elements                                 |
| `np.round`   | Round each number to the nearest integer (whole number)              |
| `np.cumprod` | A cumulative product: for each element, multiply all elements so far |
| `np.cumsum`  | A cumulative sum: for each element, add all elements so far          |
| `np.exp`     | Exponentiate each element                                            |
| `np.log`     | Take the natural logarithm of each element                           |
| `np.sqrt`    | Take the square root of each element                                 |
| `np.sort`    | Sort the elements                                                    |

| **Function**        | **Description**                                              |
| ------------------- | ------------------------------------------------------------ |
| `np.char.lower`     | Lowercase each element                                       |
| `np.char.upper`     | Uppercase each element                                       |
| `np.char.strip`     | Remove spaces at the beginning or end of each element        |
| `np.char.isalpha`   | Whether each element is only letters (no numbers or symbols) |
| `np.char.isnumeric` | Whether each element is only numeric (no letters)            |

| **Function**         | **Description**                                                                  |
| -------------------- | -------------------------------------------------------------------------------- |
| `np.char.count`      | Count the number of times a search string appears among the elements of an array |
| `np.char.find`       | The position within each element that a search string is found first             |
| `np.char.rfind`      | The position within each element that a search string is found last              |
| `np.char.startswith` | Whether each element starts with the search string                               |

当一个运算符作用在两个相同大小的 Array 上时，该运算会对 Array 中每个相应元素对进行，并以 Array 的形式返回结果。