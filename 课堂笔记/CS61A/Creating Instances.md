例子接 [[Class Statements]]
``` python
>>> a = Account("Cashrel")
```

当 Class 被调用时：
1. 创建一个新对象
2. Class 的__init__方法被调用，其中 self 参数为新对象本身，其他参数与 Class 调用时传入的参数对应。