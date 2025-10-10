#CS61B 
和 [[interface & implements]] 不同，extends 关键字主要用于拓展父类的功能。
```java
public class VengefulSLList<Item> extends SLList<Item> {
	public VengefulSLList() {
		super();
		...
	}
	...
}
```

## super
```java
Item x = super.removeLast();
...
super();
```
指向当前类的父类，可用于调用父类的成员。
继承
调用 `super()` 即调用父类的 constructor。

> [!warning] 
> 在 Java 中，子类**不会**继承父类的 constructor。同时，子类的 constructor 最开头必须调用父类的 constructor，即 super ()。不过，即使不显式地调用，Java 程序也会在开头自动地调用 super ()。

注意：Java 不支持 `super.super`
## Object
在 Java 中，所有类都是 Object 的一个后代类，未显式指定父类的类都视为 Object 的子类。

因此，Object 类所具有的所有成员都会被继承下来。如：
```java
.equals(Object obj)
.hashCode()
.toString()
```
