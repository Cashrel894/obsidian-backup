## test
test 指令将检查条件是否成立。**0=true，1=false**！务必小心！
与\[]作用相同。

test 的测试结果存储在$?中。
![[Pasted image 20250731101932.png]]
本质是$? 会存储上一条指令的 exit status。

## “boolean” ops
![[../../附件/Pasted image 20250731102325.png]]
additional:
- -a &&
- -o ||
![[../../附件/Pasted image 20250731102415.png]]

## if
语法示例：
![[../../附件/Pasted image 20250731102530.png]]
![[../../附件/Pasted image 20250731102653.png]]
![[../../附件/Pasted image 20250731102704.png]]

## case
![[../../附件/Pasted image 20250731102725.png]]


