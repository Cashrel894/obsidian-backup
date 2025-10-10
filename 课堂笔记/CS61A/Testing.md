测试不仅可以检验函数的正确性，还可以一定程度上充当 doc。

## assert
语法：assert \[Expression], \[String]
当 Expression 为假时，assert 将会抛出一个 Error，并打印 String 作为警告。
![[../../附件/Pasted image 20250730141931.png]]
编写测试函数时，应该将极端情况考虑在内。

## Doctests
在 [[Designing Functions|docstring]] 中，第一行说明函数功能后，可空一行编写 Doctest。格式如下，和 Python 交互式环境一样。
![[../../附件/Pasted image 20250730142758.png]]
可用如下指令验证 Doctest。加上 `-v` 可以让测试结果更详细。
![[../../附件/Pasted image 20250730143033.png]]
像这样只对应一个函数的测试称为**单元测试**。