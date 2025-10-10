数据抽象指一系列将多个数据复合和解构的函数。Constructor 负责复合（将数据结合为一个整体），Selector 负责解构（返回整体中的部分）。数据抽象正是通过 Constructor 和 Selector 来定义数据的行为。

> Analogously, data abstraction isolates how a compound data value is used from the details of how it is constructed.

创建一个数据抽象的一般步骤即，先设计 Constructor 和 Selector，接着再基于两者进一步抽象出其他操作。
## 抽象隔离
高层级抽象的运算只能使用对应层级的函数，而这些函数的设计只能使用低一层级的抽象。
例：
![[../../附件/Pasted image 20250804082736.png]]

好处：使程序具有更高的灵活性，如果底层的数据表示方式发生变动，上层的抽象无须修改。