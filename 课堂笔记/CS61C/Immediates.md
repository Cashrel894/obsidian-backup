#CS61C 
immediate 指代码中直接出现的常数值。

addi 可以处理包含 immediate 的加法问题，其最后一个操作数**必须是数值**，而非寄存器。
```
addi x3,x4,10   #x3 = x4 + 10;
addi x5,x6,-10  #x5 = x6 - 10;
addi x7,x0,0xff #x7 = 0xff;
```
