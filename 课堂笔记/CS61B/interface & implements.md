## interface
用于指定一个类的外部接口（即 what to do）。因此，interface 中不允许包含 private 成员，方法也只需要标识方法名和形参。
```java
public interface List<T> {
	public void addFirst(T x);
	public void addLast(T x);
	...
}
```
不过，interface 中可以指定 default 方法，所有 implements 均可使用。同时这些 default 方法也可以被子类覆写。
```java
default public void print() {
	...
}
```
而那些没有设置 default 的方法必须被子类覆写，否则会引发错误。

## implements
而 implements 则表示了这些接口的具体实现方式（即 how to do），同一接口可以有多个 implements。
```java
public class AList<T> implements List<T> {
	...
}
```

> [!note] 
> 父类类型的变量可以容纳子类类型的实例。例如 `List someList = new SLList();` 是允许的。

