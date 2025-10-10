一个对象可以具有两种字符串表示方式：
1. str 面向人类阅读者
2. repr 面向 Python 解释器
通常 str 和 repr 是相同的。
```python
repr(obj) -> string
eval(string) -> obj
eval(repr(obj)) == obj # 对于大部分对象类型成立，存在例外，如函数。
```
python 交互式环境中显示的就是对象的 repr。