#CS61C 
调用约定

用于约定在堆用函数时，哪些寄存器的值一定不会改变，而哪些则可能改变，从而减小恢复寄存器时所用的时间开销。

调用约定将所有寄存器分为两类：
1. 保留值。在调用函数前后不会改变，如 `sp gp tp s0-s11(Saved registers. s0 is also fp)` 等。
2. 非保留值。在调用函数前后可能改变。如 `a0-a7 ra t0-t6(Temporary registers)`。

![[../../附件/Pasted image 20250929124033.png]]