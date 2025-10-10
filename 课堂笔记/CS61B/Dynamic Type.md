#CS61B 
观察以下代码：
```java
public interface List {
	default public void print() {
		...
	}
	...
}

public class SLList implements List {
	@Override 
	public void print() {
		...
	}
	...
}

...

List l = new SLList();
l.print();
```

编译器只知道 `l` 是一个 `List` 类型的对象，那么它是如何知道 `l` 应该使用那个 print 方法呢？

答案是：Java 中除了变量声明时指定的**静态类型**，还有一种**动态类型**，即**运行时类型**。动态类型将告诉程序这个变量是哪个子类类型的对象，程序会优先使用动态类型 override 过的方法。

> [!warning] 
> 1. 动态类型并不适用于 overload！也就是说，当变量传入 overload 过的方法时，只会根据变量的静态类型决定所使用的方法。
> 2. 编译器在做类型检查时，也只会依据静态类型。例：对于一个在父类容器中的子类实例，调用子类中独有的成员时，编译器将报错，因为编译器只会认为是一个父类实例在调用一个对它而言不存在的成员。

动态方法选择的一般步骤：
1. 编译阶段：根据**静态类型**决定使用的方法的 Signature（方法名和形参）。
2. 运行阶段：根据**动态类型**决定激活方法的对象，并在该对象上下文中寻找 Signature 并运行。