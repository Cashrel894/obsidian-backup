#CS61C 
![[../../附件/Pasted image 20251028152249.png]]
![[../../附件/Pasted image 20251028152612.png]]
![[../../附件/Pasted image 20251028153518.png]]

## EDC/ECC
![[../../附件/Pasted image 20251028153703.png]]
![[../../附件/Pasted image 20251028155244.png]]

## Hamming ECC 
**汉明纠错码**（Hamming Error Correction Code）被广泛用于通信和存储系统中，用于提高数据的可靠性。

它在数据的 $2^n$ 位上插入纠错码，在 $2^k$ 位上的纠错码可以校验所有第 k 位为 1 的位数，（如第 4 位的纠错码可以校验 4、5、6、7、12、...）。

纠错码的校验方式是**奇偶性校验**(even parity)，即使得纠错码与其覆盖其他值的异或和为 0。

当解码汉明码时，假设至多只有一个 bit 出错，我们需要计算每一位纠错码对应的异或和，如果出现异常，就让所有出错的纠错码的位数加和，即可得到出错的哪一位，进而取反得到正确的数据。

![[../../附件/Pasted image 20251028171609.png]]

## RAID (Redundant Arrays of (Inexpensive) Disks)
![[../../附件/Pasted image 20251028171833.png]]
![[../../附件/Pasted image 20251028171954.png]]