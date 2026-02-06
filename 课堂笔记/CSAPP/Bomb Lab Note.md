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
   0x0000000000400f47 <+4>:		lea    0xc(%rsp),%rcx // rcx = rsp + 12, &b
   0x0000000000400f4c <+9>:		lea    0x8(%rsp),%rdx // rdx = rsp + 8, &a
   0x0000000000400f51 <+14>:	mov    $0x4025cf,%esi
   0x0000000000400f56 <+19>:	mov    $0x0,%eax
   0x0000000000400f5b <+24>:	call   0x400bf0 <__isoc99_sscanf@plt> // sscanf(input, "%d %d", &a, &b);
   0x0000000000400f60 <+29>:	cmp    $0x1,%eax
   0x0000000000400f63 <+32>:	jg     0x400f6a <phase_3+39>
   0x0000000000400f65 <+34>:	call   0x40143a <explode_bomb>
   0x0000000000400f6a <+39>:	cmpl   $0x7,0x8(%rsp) // 总共8个地址 switch(a)
   0x0000000000400f6f <+44>:	ja     0x400fad <phase_3+106> // default: boom
   0x0000000000400f71 <+46>:	mov    0x8(%rsp),%eax // eax = a
   0x0000000000400f75 <+50>:	jmp    *0x402470(,%rax,8) // tbl[a]
   0x0000000000400f7c <+57>:	mov    $0xcf,%eax // .case_0 eax = 0xcf
   0x0000000000400f81 <+62>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f83 <+64>:	mov    $0x2c3,%eax // .case_2 eax = 0x2c3
   0x0000000000400f88 <+69>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f8a <+71>:	mov    $0x100,%eax // .case_3 eax = 0x100
   0x0000000000400f8f <+76>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f91 <+78>:	mov    $0x185,%eax // .case_4 eax = 0x185
   0x0000000000400f96 <+83>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f98 <+85>:	mov    $0xce,%eax // .case_5 eax = 0xce
   0x0000000000400f9d <+90>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400f9f <+92>:	mov    $0x2aa,%eax // .case_6 eax = 0x2aa
   0x0000000000400fa4 <+97>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fa6 <+99>:	mov    $0x147,%eax // .case_7 eax = 0x147
   0x0000000000400fab <+104>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fad <+106>:	call   0x40143a <explode_bomb>
   0x0000000000400fb2 <+111>:	mov    $0x0,%eax
   0x0000000000400fb7 <+116>:	jmp    0x400fbe <phase_3+123>
   0x0000000000400fb9 <+118>:	mov    $0x137,%eax // .case_1 eax = 0x137
   0x0000000000400fbe <+123>:	cmp    0xc(%rsp),%eax // （此时已离开switch语句）if eax == b, goto .end, else boom.
   0x0000000000400fc2 <+127>:	je     0x400fc9 <phase_3+134>
   0x0000000000400fc4 <+129>:	call   0x40143a <explode_bomb>
   0x0000000000400fc9 <+134>:	add    $0x18,%rsp // .end
   0x0000000000400fcd <+138>:	ret
```

看到 `jmp*` 语句，基本可以断定是 `switch` 结构，不妨先看一下跳表：
```js
(gdb) x /8gx 0x402470
0x402470:	0x0000000000400f7c	0x0000000000400fb9
0x402480:	0x0000000000400f83	0x0000000000400f8a
0x402490:	0x0000000000400f91	0x0000000000400f98
0x4024a0:	0x0000000000400f9f	0x0000000000400fa6
```
依次记为 `.case_0` 到 `.case_7`，在相应语句旁添加标签。

分析发现，`phase_3` 要求输入两个 int 数据，记为 a 和 b，a 的取值只能为 0~7，a 的 8 个取值经过 `switch` 分支映射到 8 个数值，并要求输入的 b 与 a 映射后的值相等。换句话说，switch 在这里充当函数 `f(x), 0 <= x <= 7` 的作用，要求输入的 a 和 b 满足 `f(a) == b`。

阅读反汇编代码可得：
```js
x f(x)
0 0xcf
1 0x137
2 0x2c3
3 0x100
4 0x185
5 0xce
6 0x2aa
7 0x147
```
选取任意一组 `(x, f(x))` 即为答案，我选择的是 `0 207`。

## phase_4
```js
Dump of assembler code for function phase_4:
=> 0x000000000040100c <+0>:		sub    $0x18,%rsp
   0x0000000000401010 <+4>:		lea    0xc(%rsp),%rcx // rcx = rsp + 12 &b
   0x0000000000401015 <+9>:		lea    0x8(%rsp),%rdx // rdx = rsp + 8 &a
   0x000000000040101a <+14>:	mov    $0x4025cf,%esi
   0x000000000040101f <+19>:	mov    $0x0,%eax
   0x0000000000401024 <+24>:	call   0x400bf0 <__isoc99_sscanf@plt> // sscanf(input, "%d %d", &a, &b); 依然是读取两个int
   0x0000000000401029 <+29>:	cmp    $0x2,%eax
   0x000000000040102c <+32>:	jne    0x401035 <phase_4+41>
   0x000000000040102e <+34>:	cmpl   $0xe,0x8(%rsp)
   0x0000000000401033 <+39>:	jbe    0x40103a <phase_4+46> // if a > 14 or a < 0, boom.
   0x0000000000401035 <+41>:	call   0x40143a <explode_bomb>
   0x000000000040103a <+46>:	mov    $0xe,%edx // edx = 0xe
   0x000000000040103f <+51>:	mov    $0x0,%esi // esi = 0
   0x0000000000401044 <+56>:	mov    0x8(%rsp),%edi // edi = a
   0x0000000000401048 <+60>:	call   0x400fce <func4> // ret = func4(a, 0, 14)
   0x000000000040104d <+65>:	test   %eax,%eax
   0x000000000040104f <+67>:	jne    0x401058 <phase_4+76> // If ret != 0, boom
   0x0000000000401051 <+69>:	cmpl   $0x0,0xc(%rsp) // If b != 0, boom.
   0x0000000000401056 <+74>:	je     0x40105d <phase_4+81>
   0x0000000000401058 <+76>:	call   0x40143a <explode_bomb>
   0x000000000040105d <+81>:	add    $0x18,%rsp
   0x0000000000401061 <+85>:	ret
```

一眼看到 `func4`，先反汇编一下：
```js
Dump of assembler code for function func4: // (a, l, r)
   0x0000000000400fce <+0>:		sub    $0x8,%rsp
   0x0000000000400fd2 <+4>:		mov    %edx,%eax // eax = r
   0x0000000000400fd4 <+6>:		sub    %esi,%eax // eax -= l, eax = r - l
   0x0000000000400fd6 <+8>:		mov    %eax,%ecx // ecx = eax = r - l
   0x0000000000400fd8 <+10>:	shr    $0x1f,%ecx // ecx = sign_of(r - l)
   0x0000000000400fdb <+13>:	add    %ecx,%eax // eax += ecx
   0x0000000000400fdd <+15>:	sar    $1,%eax // eax >>= 1, signed. eax = ((r - l) + sign_of(r - l)) >> 1 <-> eax = (r - l) / 2
   0x0000000000400fdf <+17>:	lea    (%rax,%rsi,1),%ecx // ecx = l + (r - l) / 2, mid
   0x0000000000400fe2 <+20>:	cmp    %edi,%ecx
   0x0000000000400fe4 <+22>:	jle    0x400ff2 <func4+36> // If mid <= a, goto .le
   0x0000000000400fe6 <+24>:	lea    -0x1(%rcx),%edx // .g edx = mid - 1
   0x0000000000400fe9 <+27>:	call   0x400fce <func4> // ret = func4(a, l, mid - 1)
   0x0000000000400fee <+32>:	add    %eax,%eax // return 2 * ret
   0x0000000000400ff0 <+34>:	jmp    0x401007 <func4+57>
   0x0000000000400ff2 <+36>:	mov    $0x0,%eax // .le eax = 0
   0x0000000000400ff7 <+41>:	cmp    %edi,%ecx
   0x0000000000400ff9 <+43>:	jge    0x401007 <func4+57> // If ecx >= a, return 0
   0x0000000000400ffb <+45>:	lea    0x1(%rcx),%esi // esi = mid + 1
   0x0000000000400ffe <+48>:	call   0x400fce <func4> // ret = func4(a, mid + 1, r)
   0x0000000000401003 <+53>:	lea    0x1(%rax,%rax,1),%eax // return 2 * ret + 1
   0x0000000000401007 <+57>:	add    $0x8,%rsp // .end
   0x000000000040100b <+61>:	ret
```

发现 `func4` 会调用自身，是递归函数，需要小心分析。可以发现，`func_4` 具有明显的二分查找特征，还原为 c 代码：
```c
int func4(int a, int l, int r) {
	int mid = l + (r - l) / 2;
	
	if (mid > a) {
		return func4(a, l, mid - 1) * 2;
	}
	else if (mid == a) {
		return 0;
	}
	else {
		return func4(a, mid + 1, r) * 2 + 1;
	}
}
```

而 `phase_4` 要求 `0 <= a <= 14 && func4(a, 0, 14) == 0 && b == 0`，分析 c 语言代码不难看出 a 取 0 或 1 均满足要求。当 a 取其他值时，必然导致 `func4` 在某一次调用中走 `else` 分支，进而使得最终的返回值大于 0。

综上，答案为 `0 0` 或 `1 0`。

## phase_5
```js
Dump of assembler code for function phase_5:
=> 0x0000000000401062 <+0>:		push   %rbx
   0x0000000000401063 <+1>:		sub    $0x20,%rsp
   0x0000000000401067 <+5>:		mov    %rdi,%rbx // rbx = input
   0x000000000040106a <+8>:		mov    %fs:0x28,%rax // load canary value
   0x0000000000401073 <+17>:	mov    %rax,0x18(%rsp) // insert canary at rsp[18]
   0x0000000000401078 <+22>:	xor    %eax,%eax // eax = 0
   0x000000000040107a <+24>:	call   0x40131b <string_length>
   0x000000000040107f <+29>:	cmp    $0x6,%eax
   0x0000000000401082 <+32>:	je     0x4010d2 <phase_5+112> // If strlen(input) == 6, goto .reset_eax, else boom.
   0x0000000000401084 <+34>:	call   0x40143a <explode_bomb>
   0x0000000000401089 <+39>:	jmp    0x4010d2 <phase_5+112>
   0x000000000040108b <+41>:	movzbl (%rbx,%rax,1),%ecx // .loop ecx = (int) input[i]
   0x000000000040108f <+45>:	mov    %cl,(%rsp) // rsp[0] = (char) ecx
   0x0000000000401092 <+48>:	mov    (%rsp),%rdx // rdx = rsp[0]
   0x0000000000401096 <+52>:	and    $0xf,%edx // rdx %= 16
   0x0000000000401099 <+55>:	movzbl 0x4024b0(%rdx),%edx // rdx = *(0x4024b0 + rdx)
   0x00000000004010a0 <+62>:	mov    %dl,0x10(%rsp,%rax,1) // rsp[i + 10] = dl
   0x00000000004010a4 <+66>:	add    $0x1,%rax // i ++
   0x00000000004010a8 <+70>:	cmp    $0x6,%rax // .test_condition
   0x00000000004010ac <+74>:	jne    0x40108b <phase_5+41> // if i != 6, goto .loop
   0x00000000004010ae <+76>:	movb   $0x0,0x16(%rsp) // .loop_end rsp[16] = '\0'
   0x00000000004010b3 <+81>:	mov    $0x40245e,%esi // “flyers”
   0x00000000004010b8 <+86>:	lea    0x10(%rsp),%rdi // rdi = rsp[10] (记rsp[10]~rsp[16]代表的字符串为s)
   0x00000000004010bd <+91>:	call   0x401338 <strings_not_equal> // strings_not_equal(s, "flyers")
   0x00000000004010c2 <+96>:	test   %eax,%eax
   0x00000000004010c4 <+98>:	je     0x4010d9 <phase_5+119> // If s == "flyers", goto .s_eql, else boom.
   0x00000000004010c6 <+100>:	call   0x40143a <explode_bomb>
   0x00000000004010cb <+105>:	nopl   0x0(%rax,%rax,1)
   0x00000000004010d0 <+110>:	jmp    0x4010d9 <phase_5+119>
   0x00000000004010d2 <+112>:	mov    $0x0,%eax // .reset_eax eax = 0 i
   0x00000000004010d7 <+117>:	jmp    0x40108b <phase_5+41> // goto .loop
   0x00000000004010d9 <+119>:	mov    0x18(%rsp),%rax // .s_eql load canary value
   0x00000000004010de <+124>:	xor    %fs:0x28,%rax
   0x00000000004010e7 <+133>:	je     0x4010ee <phase_5+140> // If canary value has changed, call stack check fail.
   0x00000000004010e9 <+135>:	call   0x400b30 <__stack_chk_fail@plt>
   0x00000000004010ee <+140>:	add    $0x20,%rsp
   0x00000000004010f2 <+144>:	pop    %rbx
   0x00000000004010f3 <+145>:	ret
```

又是循环结构，阅读代码可知是一个古典加密问题。

依旧转换为 c 语言代码：
```c
void phase_5(char *input) {
	if (string_length(input) != 6) explode_bomb();
	
	char code_book[] = "maduiersnfotvbyl"; // 通过`x /s 0x4024b0`获取
	
	char decoded[7];
	for (int i = 0; i < 6; i ++) {
		decoded[i] = code_book[(int) input[i] % 16];
	}
	decoded[6] = '\0';
	
	char answer[] = "flyers"; // 通过`x /s 0x40245e`获取
	if (strings_not_equal(decoded, answer)) explode_bomb();
}
```

于是，通过 c 代码逆推可知：
```
i decoded[i] (int)input[i]%16
0 'f' 9
1 'l' 15
2 'y' 14
3 'e' 5
4 'r' 6
5 's' 7
```
只要满足 `input[i]` 的 ASCII 码模 16 与上表的值对应相等即可。

虽然可能的答案非常多，但教授们肯定在这里藏了彩蛋，所以试着构造一下可能有意义的字符串：
```ruby
irb(main):001> codes = [9, 15, 14, 5, 6, 7]
=> [9, 15, 14, 5, 6, 7]
irb(main):002> codes.map { |c| (c + 64).chr }
=> ["I", "O", "N", "E", "F", "G"]
irb(main):003> codes.map { |c| (c + 96).chr }
=> ["i", "o", "n", "e", "f", "g"]
irb(main):004> codes.map { |c| (c + 48).chr }
=> ["9", "?", ">", "5", "6", "7"]
```
好吧我可能想多了，总之纯小写字母的答案之一是 `ionefg`。

## phase_6
不是哥们怎么这么长🤔不愧是最终 Boss

我们分模块进行处理：
```js
Dump of assembler code for function phase_6:
=> 0x00000000004010f4 <+0>:		push   %r14
   0x00000000004010f6 <+2>:		push   %r13
   0x00000000004010f8 <+4>:		push   %r12
   0x00000000004010fa <+6>:		push   %rbp
   0x00000000004010fb <+7>:		push   %rbx
   0x00000000004010fc <+8>:		sub    $0x50,%rsp
   0x0000000000401100 <+12>:	mov    %rsp,%r13 // int nums[6]; int *p = nums;
   0x0000000000401103 <+15>:	mov    %rsp,%rsi
   0x0000000000401106 <+18>:	call   0x40145c <read_six_numbers> // read_six_numbers(input, nums);
   0x000000000040110b <+23>:	mov    %rsp,%r14 // r14 = nums 看起来r14是rsp的别名？暂且不知道意义何在
   0x000000000040110e <+26>:	mov    $0x0,%r12d // int i = 0
   0x0000000000401114 <+32>:	mov    %r13,%rbp // .outer_loop rbp = p
   0x0000000000401117 <+35>:	mov    0x0(%r13),%eax // x = *p
   0x000000000040111b <+39>:	sub    $0x1,%eax // x --
   0x000000000040111e <+42>:	cmp    $0x5,%eax
   0x0000000000401121 <+45>:	jbe    0x401128 <phase_6+52> // If x > 5, boom.
   0x0000000000401123 <+47>:	call   0x40143a <explode_bomb>
   0x0000000000401128 <+52>:	add    $0x1,%r12d // i ++
   0x000000000040112c <+56>:	cmp    $0x6,%r12d
   0x0000000000401130 <+60>:	je     0x401153 <phase_6+95> // If i == 6, goto .outer_loop_end
   0x0000000000401132 <+62>:	mov    %r12d,%ebx // j = i
   0x0000000000401135 <+65>:	movslq %ebx,%rax // .inner_loop x = (long) j
   0x0000000000401138 <+68>:	mov    (%rsp,%rax,4),%eax // x = nums[x]
   0x000000000040113b <+71>:	cmp    %eax,0x0(%rbp)
   0x000000000040113e <+74>:	jne    0x401145 <phase_6+81> // If p[0] == x, boom
   0x0000000000401140 <+76>:	call   0x40143a <explode_bomb>
   0x0000000000401145 <+81>:	add    $0x1,%ebx // j ++
   0x0000000000401148 <+84>:	cmp    $0x5,%ebx
   0x000000000040114b <+87>:	jle    0x401135 <phase_6+65> // If j <= 5, goto .inner_loop
   0x000000000040114d <+89>:	add    $0x4,%r13 // r13 += 4
   0x0000000000401151 <+93>:	jmp    0x401114 <phase_6+32> // goto .outer_loop
   
```


能看出有双层循环特征，尝试先把这个模块转换为 c 语言代码：
```cpp
void phase_6(char *input) {
	int nums[6];
	read_six_numbers(input, nums);
	
	/*
	for (int i = 0; i < 6; i ++) {
		if (nums[i] > 6) explode_bomb();

		int j = i + 1;
		do {
			if (nums[i] == nums[j]) explode_bomb();
			j ++;
		} while (j <= 5);
	}
	*/
	
	for (int i = 0; i < 6; i ++) {
		if (nums[i] > 6) explode_bomb();
		for (int j = i + 1; j < 6; j ++) {
			if (nums[i] == nums[j]) explode_bomb();
		}
	}
	...
}
```
经过和这坨石山代码的激烈斗争，写出了以上形式。真写出来还是比较简明易懂的，即检验 nums 中所有数均小于等于 6 且互不相等。

下一个模块：
```js
   0x0000000000401153 <+95>:	lea    0x18(%rsp),%rsi // .outer_loop_end rsi=&rsp[24]
   0x0000000000401158 <+100>:	mov    %r14,%rax // rax=r14=rsp
   0x000000000040115b <+103>:	mov    $0x7,%ecx // ecx=7
   0x0000000000401160 <+108>:	mov    %ecx,%edx // .loop edx=ecx
   0x0000000000401162 <+110>:	sub    (%rax),%edx // rdx-=*rax
   0x0000000000401164 <+112>:	mov    %edx,(%rax) // *rax=rdx
   0x0000000000401166 <+114>:	add    $0x4,%rax //rax+=4
   0x000000000040116a <+118>:	cmp    %rsi,%rax
   0x000000000040116d <+121>:	jne    0x401160 <phase_6+108> // If rax != rsi, goto .loop
```
看起来是一个小循环，翻译为 c 代码：
```c
void phase_6(char *input) {
	...
	for (int i = 0; i < 6; i ++) {
		nums[i] = 7 - nums[i];
	}
	...
}
```
这部分相对简单。


下一个：
```js
   0x000000000040116f <+123>:	mov    $0x0,%esi // esi=0
   0x0000000000401174 <+128>:	jmp    0x401197 <phase_6+163> // goto .conditon
   0x0000000000401176 <+130>:	mov    0x8(%rdx),%rdx // .loop rdx = *(rdx + 8)
   0x000000000040117a <+134>:	add    $0x1,%eax // eax ++
   0x000000000040117d <+137>:	cmp    %ecx,%eax
   0x000000000040117f <+139>:	jne    0x401176 <phase_6+130> // If eax != ecx, continue
   0x0000000000401181 <+141>:	jmp    0x401188 <phase_6+148>
   0x0000000000401183 <+143>:	mov    $0x6032d0,%edx // edx = 0x6032d0
   0x0000000000401188 <+148>:	mov    %rdx,0x20(%rsp,%rsi,2) // rsp[20+rsi*2] = rdx
   0x000000000040118d <+153>:	add    $0x4,%rsi // rsi += 4
   0x0000000000401191 <+157>:	cmp    $0x18,%rsi
   0x0000000000401195 <+161>:	je     0x4011ab <phase_6+183> // if eax == ecx, break
   0x0000000000401197 <+163>:	mov    (%rsp,%rsi,1),%ecx // .condtion ecx = nums[rsi]
   0x000000000040119a <+166>:	cmp    $0x1,%ecx
   0x000000000040119d <+169>:	jle    0x401183 <phase_6+143> // If ecx <= 1, goto 143
   0x000000000040119f <+171>:	mov    $0x1,%eax // eax = 1
   0x00000000004011a4 <+176>:	mov    $0x6032d0,%edx // edx = 0x6032d0
   0x00000000004011a9 <+181>:	jmp    0x401176 <phase_6+130> // goto .loop
```
`0x6032d0` 处存的似乎是链表的六个结点，打印出来看看：
```js
(gdb) x /24x 0x6032d0
0x6032d0 <node1>:	0x0000014c	0x00000001	0x006032e0	0x00000000
0x6032e0 <node2>:	0x000000a8	0x00000002	0x006032f0	0x00000000
0x6032f0 <node3>:	0x0000039c	0x00000003	0x00603300	0x00000000
0x603300 <node4>:	0x000002b3	0x00000004	0x00603310	0x00000000
0x603310 <node5>:	0x000001dd	0x00000005	0x00603320	0x00000000
0x603320 <node6>:	0x000001bb	0x00000006	0x00000000	0x00000000
```
猜测每个结点的第二个值是结点编号 `id`，最后一个值为结点的 ` next ` 字段，第一个暂时不明，姑且认为是 data。可以写出结构体定义：
```c
struct node {
	int data;
	int id;
	node *next;
}
```

看出是链表后思路要清晰一点了：
```c
node ns[6]; // 定义省略
void phase_6(char *input) {
	...
	node* nps[N]; // 0x20
	
	for (int i = 0; i < 6; i ++) {
		node *d = ns;
		for (int j = 1; j < nums[i]; j ++) {
			d = d->next;
		}
		nps[i] = d;
	}
	...
}
```

下一段循环：
```js
   0x00000000004011ab <+183>:	mov    0x20(%rsp),%rbx // rbx=*(rsp+20)=nps[0]
   0x00000000004011b0 <+188>:	lea    0x28(%rsp),%rax // rax=rsp+28=&nps[1]
   0x00000000004011b5 <+193>:	lea    0x50(%rsp),%rsi
   0x00000000004011ba <+198>:	mov    %rbx,%rcx // rcx=rbx=nps[0]
   0x00000000004011bd <+201>:	mov    (%rax),%rdx // .loop rdx=nps[i]
   0x00000000004011c0 <+204>:	mov    %rdx,0x8(%rcx) // rcx->next=rdx
   0x00000000004011c4 <+208>:	add    $0x8,%rax // rax+=8
   0x00000000004011c8 <+212>:	cmp    %rsi,%rax
   0x00000000004011cb <+215>:	je     0x4011d2 <phase_6+222> // If rax==rsi, break
   0x00000000004011cd <+217>:	mov    %rdx,%rcx // rcx=rdx
   0x00000000004011d0 <+220>:	jmp    0x4011bd <phase_6+201> // goto .loop
   0x00000000004011d2 <+222>:	movq   $0x0,0x8(%rdx)
```

翻译为 c 代码：
```c
void phase_6(char *input) {
	...
	for (int i = 1; i < 6; i ++) {
		nps[i - 1]->next = nps[i];
	}
	nps[5]->next = NULL;
	...
}
```
到目前为止的代码，主要做的就是根据输入的六个数字，将链表重新排序。

```js
   0x00000000004011da <+230>:	mov    $0x5,%ebp // ebp = 5
   0x00000000004011df <+235>:	mov    0x8(%rbx),%rax // rax = rbx->next
   0x00000000004011e3 <+239>:	mov    (%rax),%eax // rax = rax->data
   0x00000000004011e5 <+241>:	cmp    %eax,(%rbx) // If rbx->data < rax->data, boom. 进一步展开，其实就是要验证rbx->data >= rbx->next->data。这里其实就可以猜出来这段代码是干啥的了
   0x00000000004011e7 <+243>:	jge    0x4011ee <phase_6+250>
   0x00000000004011e9 <+245>:	call   0x40143a <explode_bomb>
   0x00000000004011ee <+250>:	mov    0x8(%rbx),%rbx
   0x00000000004011f2 <+254>:	sub    $0x1,%ebp
   0x00000000004011f5 <+257>:	jne    0x4011df <phase_6+235>
   0x00000000004011f7 <+259>:	add    $0x50,%rsp
   0x00000000004011fb <+263>:	pop    %rbx
   0x00000000004011fc <+264>:	pop    %rbp
   0x00000000004011fd <+265>:	pop    %r12
   0x00000000004011ff <+267>:	pop    %r13
   0x0000000000401201 <+269>:	pop    %r14
   0x0000000000401203 <+271>:	ret
```

翻译为 c 代码：
```c
void phase_6(char *input) {
	...
	node *cur = nps[0];
	for (int i = 0; i < 5; i ++) {
		if (cur->data < cur->next->data) explode_bomb();
		cur = cur->next;
	}
}
```
这段代码要求我们保证重排后的链表满足 data 字段单调递减。

到此全部代码翻译完毕，下面开始逆推。通过比较 data 字段大小，重排后的链表应为：
```
3 -> 4 -> 5 -> 6 -> 1 -> 2
```

再回味之前重排部分的代码可知，要求的输入序列恰好就是上述结点 id 的顺序！……才怪。注意我们中间有修改过 nums 的值：
```
nums[i] = 7 - nums[i];
```

所以这一步逆推也要考虑上，答案应为：
```
4 3 2 1 6 5
```

## The End
完结撒炸弹！
```
(gdb) run psol.txt 
Starting program: /home/cashrel/Cashrel/wp/csapp/labs/bomblab/bomb/bomb psol.txt
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Welcome to my fiendish little bomb. You have 6 phases with
which to blow yourself up. Have a nice day!
Phase 1 defused. How about the next one?
That's number 2.  Keep going!
Halfway there!
So you got that one.  Try this one.
Good work!  On to the next...
Congratulations! You've defused the bomb!
[Inferior 1 (process 343119) exited normally]
```