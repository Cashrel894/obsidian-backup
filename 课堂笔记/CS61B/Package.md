#CS61B 
在 java 中，使用包 (package) 来分离代码功能和代码的实际实现逻辑。

可以文件开头使用 `package <name>;` 来指定当前文件为某个包的一部分，使用时需用点表示法索引，或者 `import` 相应的包。

```java
org.junit.Assert.assertEquals(a, b);
```
其中，org.junit 为包名，Assert 为类名，而 assertEquals 则为 Assert 类的一个方法。

> [!note] 
> 通常来讲，包名来源于开发该包的个人/组织的互联网地址，但会反过来写。例如，org.junit 包就被维护在 junit.org 上。
