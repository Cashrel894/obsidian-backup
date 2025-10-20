#CS61C 
缓存是介于 [[Registers]] 和内存之间的存储元件，它有着相比于 [[Registers]] 更大的存储空间(10^4 级别的字节数)，同时相比于内存更快的访问速度（1 个时钟周期）。

缓存的设计原则是：**时间局部性**和**空间局部性**
- 如果现在使用了一块内存，那么很有可能我们很快还需要使用同一块内存。
- 如果使用了一块内存，那么很有可能我们还需要访问其邻近的其他内存。

## TIC Cache Mnemonic
![[../../附件/Pasted image 20251018174205.png]]

![[../../附件/Pasted image 20251018174441.png]]

## Cache Terminology
![[../../附件/Pasted image 20251019215536.png]]
![[../../附件/Pasted image 20251019215627.png]]
![[../../附件/Pasted image 20251019215657.png]]
![[../../附件/Pasted image 20251019220133.png]]

## Write Back 
![[../../附件/Pasted image 20251020131538.png]]

## Block Size Tradeoff 
![[../../附件/Pasted image 20251020131615.png]]
![[../../附件/Pasted image 20251020131630.png]]
## Cache Misses 
![[../../附件/Pasted image 20251020131340.png]]
![[../../附件/Pasted image 20251020131349.png]]
![[../../附件/Pasted image 20251020132429.png]]
![[../../附件/Pasted image 20251020132553.png]]
## Fully Associative Caches
![[../../附件/Pasted image 20251020132240.png]]
![[../../附件/Pasted image 20251020132251.png]]
![[../../附件/Pasted image 20251020132347.png]]

## Set Associative Cache 
![[../../附件/Pasted image 20251020133006.png]]
![[../../附件/Pasted image 20251020133131.png]]
![[../../附件/Pasted image 20251020133209.png]]
![[../../附件/Pasted image 20251020133234.png]]

## Block Replacement Policy
![[../../附件/Pasted image 20251020133623.png]]
![[../../附件/Pasted image 20251020133648.png]]

## Average Memory Access Time (AMAT)
![[../../附件/Pasted image 20251020134223.png]]
![[../../附件/Pasted image 20251020134236.png]]
![[../../附件/Pasted image 20251020134524.png]]
![[../../附件/Pasted image 20251020134635.png]]
![[../../附件/Pasted image 20251020134722.png]]

## Conclusion 
![[../../附件/Pasted image 20251020135500.png]]
