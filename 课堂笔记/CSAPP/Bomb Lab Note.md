#csapp 
本笔记主要记录完成 Bomb Lab 全过程，包含所用命令、破解思路以及临时草稿。

## 准备
运行环境：
- Ubuntu 24.04 LTS 发行版
- zsh 终端

打开 https://csapp.cs.cmu.edu/3e/labs.html ，找到 Bomb Lab 段落，阅读 [Writeup](http://csapp.cs.cmu.edu/3e/bomblab.pdf)，下载 https://csapp.cs.cmu.edu/3e/bomb.tar 并解压。

解压得到的文件夹下包含 `bomb` 可执行文件以及 `bomb.c` 源代码，`bomb.c` 主要交代一些该 lab 的元数据以及神秘小剧情，此外我们知道炸弹的六个阶段分别调用 `phase_1` 到 `phase_6` 函数，需要在这些函数打断点。

接下来直奔主题，`gdb bomb` 开始调试。

## phase_1
`disassemble phase_1`，发现代码通过 `explode_bomb` 函数引爆炸弹，直接 `break explode_bomb` 上个保险。

`run`，抵达 `phase_1` 断点，`disassemble`，结果如下：
```js
Dump of assembler code for function phase_1:
=> 0x0000000000400ee0 <+0>:	    sub    $0x8,%rsp
   0x0000000000400ee4 <+4>:	    mov    $0x402400,%esi
   0x0000000000400ee9 <+9>:	    call   0x401338 <strings_not_equal>
   0x0000000000400eee <+14>:	test   %eax,%eax
   0x0000000000400ef0 <+16>:	je     0x400ef7 <phase_1+23>
   0x0000000000400ef2 <+18>:	call   0x40143a <explode_bomb>
   0x0000000000400ef7 <+23>:	add    $0x8,%rsp
   0x0000000000400efb <+27>:	ret
```
阶段 1 很简单，就是对比 `input` 和 `0x402400` 处存储的字符串是否相等，若不相等就引爆炸弹。

检查该字符串：
```js
(gdb) x /s 0x402400
0x402400:	"Border relations with Canada have never been better."
```

创建一个新文件 `psol.txt`，存入 `Border relations with Canada have never been better.`，再次打开 gdb，输入 `run psol.txt`，成功通过阶段 1。以后每个阶段的答案都会存入 `psol.txt`，这样就不需要手动输入已通过阶段的答案了。

## phase_2
反汇编 `phase_2`:
```js
Dump of assembler code for function phase_2:
=> 0x0000000000400efc <+0>:	    push   %rbp
   0x0000000000400efd <+1>:	    push   %rbx
   0x0000000000400efe <+2>:	    sub    $0x28,%rsp
   0x0000000000400f02 <+6>:	    mov    %rsp,%rsi // rsi = rsp
   0x0000000000400f05 <+9>:	    call   0x40145c <read_six_numbers> // read_six_numbers(input, %rsp); 不妨记这六个int依次存储在a[0]~a[5]中
   0x0000000000400f0a <+14>:	cmpl   $0x1,(%rsp) 
   0x0000000000400f0e <+18>:	je     0x400f30 <phase_2+52> // If a[0] == 1, goto .a0_1, else boom.
   0x0000000000400f10 <+20>:	call   0x40143a <explode_bomb>
   0x0000000000400f15 <+25>:	jmp    0x400f30 <phase_2+52> // goto .a0_1
   0x0000000000400f17 <+27>:	mov    -0x4(%rbx),%eax // .loop eax = a[rbx - 1]
   0x0000000000400f1a <+30>:	add    %eax,%eax // eax *= 2
   0x0000000000400f1c <+32>:	cmp    %eax,(%rbx)
   0x0000000000400f1e <+34>:	je     0x400f25 <phase_2+41> // If a[rbx] == a[rbx - 1] * 2, goto .a0_e_a1, else boom
   0x0000000000400f20 <+36>:	call   0x40143a <explode_bomb>
   0x0000000000400f25 <+41>:	add    $0x4,%rbx // .a0_e_a1 rbx += 4
   0x0000000000400f29 <+45>:	cmp    %rbp,%rbx
   0x0000000000400f2c <+48>:	jne    0x400f17 <phase_2+27> // If rbx != &a[7], goto .loop
   0x0000000000400f2e <+50>:	jmp    0x400f3c <phase_2+64> // goto .end
   0x0000000000400f30 <+52>:	lea    0x4(%rsp),%rbx // .a0_1 rbx = &a[1]
   0x0000000000400f35 <+57>:	lea    0x18(%rsp),%rbp // rbp = &a[7]
   0x0000000000400f3a <+62>:	jmp    0x400f17 <phase_2+27> // goto .loop
   0x0000000000400f3c <+64>:	add    $0x28,%rsp // .end
   0x0000000000400f40 <+68>:	pop    %rbx
   0x0000000000400f41 <+69>:	pop    %rbp
   0x0000000000400f42 <+70>:	ret
```

推测 `read_six_numbers` 将 `input` 读取为 6 个数值，存到栈空间内，以防万一 `disassemble` 一下：
```js
Dump of assembler code for function read_six_numbers: // (input, dest)
   0x000000000040145c <+0>:		sub    $0x18,%rsp
   0x0000000000401460 <+4>:		mov    %rsi,%rdx // rdx = dest
   0x0000000000401463 <+7>:		lea    0x4(%rsi),%rcx // rcx = dest + 4
   0x0000000000401467 <+11>:	lea    0x14(%rsi),%rax // rax = dest + 20
   0x000000000040146b <+15>:	mov    %rax,0x8(%rsp) // *(rsp + 8) = rax = dest + 20
   0x0000000000401470 <+20>:	lea    0x10(%rsi),%rax // rax = dest + 16
   0x0000000000401474 <+24>:	mov    %rax,(%rsp) // *(rsp) = rax = dest + 16
   0x0000000000401478 <+28>:	lea    0xc(%rsi),%r9 // r9 = dest + 12
   0x000000000040147c <+32>:	lea    0x8(%rsi),%r8 // r8 = dest + 8
   0x0000000000401480 <+36>:	mov    $0x4025c3,%esi // rsi = 0x4025c3 ("%d %d %d %d %d %d")，即读取六个由空白字符分隔的int。
   0x0000000000401485 <+41>:	mov    $0x0,%eax // rax = 0
   0x000000000040148a <+46>:	call   0x400bf0 <__isoc99_sscanf@plt> // sscanf(input, "%d %d %d %d %d %d", dest, dest + 4, dest + 8, dest + 12, dest + 16, dest + 20)
   0x000000000040148f <+51>:	cmp    $0x5,%eax
   0x0000000000401492 <+54>:	jg     0x401499 <read_six_numbers+61>
   0x0000000000401494 <+56>:	call   0x40143a <explode_bomb> // 若读取失败，引爆
   0x0000000000401499 <+61>:	add    $0x18,%rsp
   0x000000000040149d <+65>:	ret
```
看得出来，确实就是从低位到高位逐个存储读取到的 6 个 int 数据。

继续分析 `phase_2` 汇编代码，发现是很明显的 for 循环结构，并使用指针遍历，其检查的无非就是两个条件：
1. `a[0] == 1`
2. `a[i] == a[i - 1] * 2 for all 1 <= i <= 5`

因此，该阶段答案应为 `1 2 4 8 16 32`。

## phase_3
```js
Dump of assembler code for function phase_3:
=> 0x0000000000400f43 <+0>:		sub    $0x18,%rsp
   0x0000000000400f47 <+4>:		lea    0xc(%rsp),%rcx
   0x0000000000400f4c <+9>:		lea    0x8(%rsp),%rdx
   0x0000000000400f51 <+14>:	mov    $0x4025cf,%esi
   0x0000000000400f56 <+19>:	mov    $0x0,%eax
   0x0000000000400f5b <+24>:	call   0x400bf0 <__isoc99_sscanf@plt>
   0x0000000000400f60 <+29>:	cmp    $0x1,%eax
   0x0000000000400f63 <+32>:	jg     0x400f6a <phase_3+39>
   0x0000000000400f65 <+34>:	call   0x40143a <explode_bomb>
   0x0000000000400f6a <+39>:	cmpl   $0x7,0x8(%rsp)
   0x0000000000400f6f <+44>:	ja     0x400fad <phase_3+106>
   0x0000000000400f71 <+46>:	mov    0x8(%rsp),%eax
   0x0000000000400f75 <+50>:	jmp    *0x402470(,%rax,8)
   0x0000000000400f7c <+57>:	mov    $0xcf,%eax
   0x0000000000400f81 <+62>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f83 <+64>:	mov    $0x2c3,%eax
   0x0000000000400f88 <+69>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f8a <+71>:	mov    $0x100,%eax
   0x0000000000400f8f <+76>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f91 <+78>:	mov    $0x185,%eax
   0x0000000000400f96 <+83>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f98 <+85>:	mov    $0xce,%eax
   0x0000000000400f9d <+90>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f9f <+92>:	mov    $0x2aa,%eax
   0x0000000000400fa4 <+97>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fa6 <+99>:	mov    $0x147,%eax
   0x0000000000400fab <+104>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fad <+106>:	call   0x40143a <explode_bomb>
   0x0000000000400fb2 <+111>:	mov    $0x0,%eax
   0x0000000000400fb7 <+116>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fb9 <+118>:	mov    $0x137,%eax
   0x0000000000400fbe <+123>:	cmp    0xc(%rsp),%eax
   0x0000000000400fc2 <+127>:	je     0x400fc9 <phase_3+134>
   0x0000000000400fc4 <+129>:	call   0x40143a <explode_bomb>
   0x0000000000400fc9 <+134>:	add    $0x18,%rsp
   0x0000000000400fcd <+138>:	ret
```

看到 `jmp*` 语句，基本可以断定是 `switch` 结构。