#CS61C 
链接器将多个 Object File 合并在一起，产生可执行文件(.out)：
1. 将每个 .o 文件的 text 字段合并
2. 将每个 .o 文件的 data 字段合并
3. 解引用，即遍历 Relocation Information，用绝对地址填充待处理的信息。[[Assembler]]
![[../../附件/Pasted image 20251002091826.png]]

哪些信息是需要链接器来处理的呢？
- 外部的函数引用（如 jal 和 auipc/jalr）：因为在汇编时这些引用的地址是未知的。
- 静态数据引用（lw, sw, lui/addi）：因为 Data 字段在链接时移动了位置。


在链接外部函数时，如果函数属于库文件 （library files），那么若链接器直接将所有库文件都包含在可执行文件中，即使很多库函数都没有被使用，这种方式被称作 **Statically-Linked Libraries**。

还有一种常见的链接方式，称为 **Dynamically-Linked Libraries**，简称 **DLLs**。它将库文件存储在 .dll 文件中，程序运行时可以根据需要可以动态加载这些文件，一方面减少存储浪费，另一方面方便更新库函数。但缺点是容易导致依赖问题，同时也会增加运行时加载文件导致的时间开销。