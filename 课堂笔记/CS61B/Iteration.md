#CS61B 
```java
for (int i : arr) {
	System.out.println(i);
}
```
对于我们自己创建的类，我们也有办法像遍历数组一样方便地遍历实例中包含的元素。

## iterator
若要做到这一点，我们需要使对象能够返回一个迭代器。这个迭代器需要包含一些必要的方法：
```java
boolean hasNext(); // 如果有下一个元素，返回true，否则返回false
T next(); // 返回下一个元素
```
而这个迭代器应当继承自  `Iterator<T>`：
```java
private class myIterator implements Iterator<T> {
	...
}
```
同时，这个迭代器需要被我们的类的 `public Iterator<T> iterator()` 方法返回：
```java
public Iterator<T> iterator() {
	return new myIterator();
}
```
我们自己的类也需要成为 `Iterable<T>` 类的子类：
```java
public class myClass implements Iterable<T> {
	...
}
```
这样一来，我们就可以用类似开头的方法遍历对象了。
实际上，开头的语法基本等价于：
```java
Iterator<int> t = arr.iterator();
while (t.hasNext()) {
	int i = t.next();
	System.out.println(i);
}
```