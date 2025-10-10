#CS61C 
CPU: Central Processing Unit 

CPU 是一台电脑的核心，负责数据操作和决策功能。CPU 由两大部分组成：
- Datapath：是 CPU 的“肌肉”，包含了处理器执行操作所必要的硬件。
- Control：是 CPU 的“大脑”，指挥 Datapath 执行操作。
![[../../附件/Pasted image 20251007210242.png]]

## CPU Elements & Stages 
CPU 由两种子电路组成：组合逻辑块（combinational logic block）和状态组件（state elements）。

### One Instruction Per Cycle
在 CPU 时钟的 tick 中，计算机是这样执行一个指令的：
- 将状态组件的当前输出作为组合逻辑块的输入。
- 在下一个 tick 之前，将组合逻辑块的输出设置为状态组件的输入。

接着，在 tock 和下一个 tick 之间，即**上升沿**（rising clock edge）：
- 所有的状态组件的输出被更新，下一个时钟周期开始。

### State Elements 
状态组件主要有三个：
- PC (Program Counter) ![[../../附件/Pasted image 20251007211818.png]]
- Register ![[../../附件/Pasted image 20251007212009.png]]
- Memory ![[../../附件/Pasted image 20251007212105.png]] ![[../../附件/Pasted image 20251007212317.png]]

### 5 Basic Stages of Instruction Execution 
![[../../附件/Pasted image 20251007212953.png]]
> [!note] 
> 并不是所有的操作都需要涉及完整的五个操作。例：write enable / MUX selector 

## Summary I
![[../../附件/Pasted image 20251008215037.png]]

## Complete  Datapath
![[../../附件/Pasted image 20251009130012.png]]
