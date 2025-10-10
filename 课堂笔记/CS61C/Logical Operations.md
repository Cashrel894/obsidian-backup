#CS61C 
```
and dr,r1,r2
andi dr,r1,imm
or dr,r1,r2
ori dr,r1,imm
xor dr,r1,r2
xori dr,r1,imm
sll dr,r1,r2 #Shift Left Logical. = '<<' in C
slli dr,r1,imm
srl dr,r1,r2 #Shift Right Logical. = '>>' in C
srli dr,r1,imm
sra dr,r1,r2 #Shift Right Arithmetic.
srai dr,r1,imm
```
> [!note] 
> srl 与 sra 的区别：
> 数位移动时，高位会空缺，srl 会用 0 来填充高位，而 sra 则是用符号位。


