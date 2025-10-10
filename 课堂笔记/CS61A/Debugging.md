当出现 Error 时，控制台会输出 traceback，以帮助程序员定位 Error 出现的位置。

## Traceback Messages
分为两行，第一行格式如下：
`File "<file name>", line <number>, in <function>`
第二行则是引发 Error 的函数调用/表达式。
按照函数调用的先后顺序显示，最后调用的将在最底部出现。
![[../../附件/Pasted image 20250730202812.png]]
## Error Messages
格式如下：
`<error type>: <error message>`
![[../../附件/Pasted image 20250730203107.png]]

## Error Types
- SyntaxError
- IndentationError
- TypeError
- NameError
- IndexError