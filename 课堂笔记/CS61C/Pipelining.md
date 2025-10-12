#CS61C 
## Pipelining RISC-V Instructions
对于每条指令的**流水线**式安排可以显著优化运行效率。

![[../../附件/Pasted image 20251012124245.png]]

如图，在同一时间内，不同指令可以利用 CPU 的不同部分完成不同阶段，从而充分利用 CPU 资源。

单条指令完成的时间依然不变，但是所有指令的总时间却能够大大缩短。

而对于每一个时间段，完成阶段所需时间总是**取决于最慢的**那一个。

此外，在指令开始和结束的时候，会出现同一时间段只有部分阶段正在进行的情况，分别被称为"Filling"和"Draining"。

**时延（Latency）**，即完成单条指令的时间，不会改变。而**吞吐量（Throughput）** 则会大大增加，增加量与指令阶段总数正相关，但要注意 Filling 和 Draining 会造成限制。

## Pipelined Datapath
为了支持流水线，[[RISC-V Single-Cycle Datapath]] 需要在硬件上将整个 datapath 分为 5 个阶段，每个阶段之间加上临时寄存器供下个时钟周期使用。
![[../../附件/Pasted image 20251012125517.png]]

注意到 PC 和 PC+4 都用了一系列寄存器传递，这其实是不必要的，可以在 MEM 阶段重新计算+4，因此可以作出优化：
![[../../附件/Pasted image 20251012130021.png]]

此外 [[RISC-V Single-Cycle Control]] 也需要适配流水线，因此：
![[../../附件/Pasted image 20251012130131.png]]

又由于寄存器的重新写入发生在 WB 阶段，RegWriteIndex 导入的 inst 应该从 WB 阶段获取：
![[../../附件/Pasted image 20251012130322.png]]

不过关于 [[RISC-V Single-Cycle Control]] 还有一定的问题，需要解决不同指令的 control logic 不同带来的问题，可能的方案如下：
![[../../附件/Pasted image 20251012130512.png]]