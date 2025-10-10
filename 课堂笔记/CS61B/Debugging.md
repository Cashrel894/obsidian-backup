#CS61B 
## Java Debugger
### Breaking Points
点击代码所在行号，代码左侧会出现一个红点，称为“断点”。调试程序时，程序会先一直执行，直到遇到断点时停下。

此外，还可以为断点设置 Condition，程序只在 Condition 为 true 时在此行停止。

### Step Into
而 Step Into 可以逐行执行下一行代码。注意调试器高亮的行为**即将执行**的行，而非已经执行。

### Step Over & Step Out 
Step Into 会执行 literally 下一步程序，而 Step Over 会跳过函数的内部执行，实现抽象。

Step Out 则直接跳出当前函数的调试。

![[../../附件/Pasted image 20250820213205.png]]
![[../../附件/Pasted image 20250820213423.png]]

## Errors
![[../../附件/Pasted image 20250820212526.png]]