#CS61C 
在 RISC-V 中也可以实现分支结构：
```
beg reg1,reg2,L1
```
beg: branch if equal
意思是：如果 reg 1 与 reg 2 的内容相等，那么跳转到标签为 L 1 的指令；否则，执行下一条指令。

除此之外还有：
- bne: branch if not equal
- blt: branch if less than
- bge: branch if greater than
- 此外 blt 和 bge 还有对应的无符号版本：bltu bgeu
- **不存在** ble 和 bgt

以及无条件的指令跳转：`j label`
