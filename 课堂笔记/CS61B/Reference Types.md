#CS61B 
除了 [[Primitive Types]] 外， Java 还有第九种数据类型：Object。

当我们实例化一个对象时：
1. Java 会为实例的每个变量分配空间并填充默认值（如 0、null）
2. 调用 constructor 更改实例变量值。
3. **new** 关键字返回实例所在地址（64 bit）。

当声明一个 Reference Type 时：
1. Java 会分配一个 64 bit 的空间，无论是哪种 Object
2. 这些 bit 初始会被设置为 null，随后可以被赋值为实例的地址。