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
![[../../附件/Pasted image 20251013145450.png]]

### Sol 2: Forwarding 
增加硬件，在前一条指令 WB 之前，就先把结果传递到前面的阶段。

实际上，对于大部分指令，我们在 EX 阶段就已经知道了需要写入的结果，所以可以通过硬件设施将结果传递到后一条指令的 EX 阶段。
![[../../附件/Pasted image 20251013144614.png]]

这种方法需要增加额外的 datapath 和判断逻辑：
![[../../附件/Pasted image 20251013144719.png]]

![[../../附件/Pasted image 20251013144804.png]]

特别地，lw 指令在 MEM 阶段才能获取结果，直接传递结果来不及，只能采用 Sol 1，Stall 一个周期。
![[../../附件/Pasted image 20251013145542.png]]
然后对于 MEM 阶段也需要额外硬件设施：
![[../../附件/Pasted image 20251013145630.png]]
判断逻辑由 [[RISC-V Single-Cycle Control]] 完成：
![[../../附件/Pasted image 20251013145716.png]]
### Sol 3: Code Scheduling
Stalling 由于插入了 no-op，造成了性能损失。

可以让编译器重新安排指令顺序，在 no-op 的位置放置无关指令，来避免发生 Data Hazards 

![[../../附件/Pasted image 20251013145921.png]]

## Control Hazards 
当使用 branch 指令时，我们要到 EX 阶段才能获知 PC 要跳转到的位置，但这个时候已经有两条指令开始，怎么办？
![[../../附件/Pasted image 20251013150448.png]]

当 branch 分支成功时，后面两条指令实际上不被需要，应该被转换为 no-op 被流水线剔除，这个过程被称为 *flushing* the pipeline。

但问题来了：这样的话，每次 flush 都会导致三个循环的 no-op。（虽然分支的结果在 EX 就知道了，但 PC 的写回发生在 MEM 阶段，所以是 3 个周期）

解决方案是**分支预测**（Branch Prediction）。我们需要猜测哪个分支会成功，并将对应分支的两条指令加载进来。但若猜测失败，则需要付出 flushing 的代价。

例：总是猜测分支会失败
![[../../附件/Pasted image 20251013151419.png]]

## Superscalar Processors
![[../../附件/Pasted image 20251013152415.png]]
![[../../附件/Pasted image 20251013152426.png]]
![[../../附件/Pasted image 20251013152636.png]]
![[../../附件/Pasted image 20251013153222.png]]
![[../../附件/Pasted image 20251013153229.png]]