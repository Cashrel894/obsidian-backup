#CS61C 
## Structural Hazards
Problem 1: 多条指令需要同时访问同一资源。即，同一资源在不同阶段中都可能被使用。
Solution 1: 设计好 ISA（如 RISC-V），防止指令冲突
其他解决方案：增加每条指令的时钟周期数，轮流使用资源，没轮到的指令必须等待；增加资源数等。

Problem 2: 当 IF 使用 IMEM 时，MEM 也在使用 DMEM，而实际上 IMEM 和 DMEM 来自同一块内存。
Solution 2: 使用**缓存**[[Caches]] 分离 IMEM 和 DMEM。

## Data Hazards
Problem：如果前一条指令在 WB 更新了资源，但后一条指令访问资源所处的 ID 发生在前一条的 WB 之前，导致前一条指令的更新无法被后一条指令使用。
![[../../附件/Pasted image 20251013143728.png]]

### Sol 1: Stalling
后一条指令在前一条指令 WB 结束后再 ID。具体实现方式是，在两条可能冲突的指令之间插入 no-op（无实际效果的指令，如 `addi x0 x0 0`），在 RISC-V 中用伪代码 `nop` 表示。

可以由编译器完成，但这要求编译器了解流水线架构。

### Sol 2: Forwarding 
增加硬件，在前一条指令 WB 之前，就先把结果传递到前面的阶段。

实际上，我们在 EX 阶段就已经知道了需要写入的结果，所以可以通过硬件设施将结果传递到后一条指令的 EX 阶段。
![[../../附件/Pasted image 20251013144614.png]]

这种方法需要增加额外的 datapath 和判断逻辑：
![[../../附件/Pasted image 20251013144719.png]]

![[../../附件/Pasted image 20251013144804.png]]

### Sol 3: Code Scheduling
重新安排指令顺序，避免出现 Data Hazards

