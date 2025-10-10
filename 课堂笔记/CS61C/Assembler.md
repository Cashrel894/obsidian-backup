#CS61C 
![[../../附件/Pasted image 20251002084937.png]]

汇编器使用 directive，汇编产生 Object Code（机器语言）（.o 文件）以及链接和调试所需的信息。

所谓 directive，通常由编译器产生（.s 文件），用于指示构建 object code 的不同部分的方式。
![[../../附件/Pasted image 20251002085347.png]]

## Object File Format
1. Object File Header: 文件大小，以及其他 Object File 的位置。
2. Text Segment: 机器代码
	1. 过程中需要将 [[J-Format]] 与 [[B-Format]] 伪代码翻译，并将标签替换为相对偏置（half word 为单位）。
		1. 对于在同一文件的标签，替换前需要先遍历代码，在 Symbol Table 中记录标签对应的指令地址，再一次遍历进行偏置的计算和替换。
		2. 如果需要引用其他文件或者静态数据，那么就需要用到 Relocation Information 和 Symbol Table。
3. Data Segment: 源文件中静态数据的二进制表示。
4. Symbol Table: 存储了文件的标签以及静态数据。
	1. 有 .globl directive 的标签可以被其他文件引用。
	2.  静态数据即任何在 .data 区域的数据。
5. Relocation Information: 存储了那些因引用了其他文件或静态数据，导致汇编器暂时无法处理的指令，用于链接器进一步替换。
6. Debugging Information
