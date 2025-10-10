#CS61B 
有时，我们可以利用继承，为多个类提供通用的方法，以提高代码的复用率，称为“子类多态”。

```java
public interface OurComparable {
	public int compareTo(Object o);
}

public class Dog implements OurComparable {
	private int size;
	
	public int compareTo(Object o) {
		Dog uddaDog = (Dog) o;
		return this.size - o.size;
	}
}

public class Maximizer{
	public static OurComparable max(OurComparable[] items) {
		int maxDex = 0;
		for (int i = 0; i < items.length; i ++) {
			int cmp = items.compareTo(items[maxDex]);
			if (cmp > 0) {
				maxDex = i;
			}
		}
	}
	return items[maxDex];
}
```

还有一种更优雅的实现方式，可以无须 [[Casting]] 特定类型：
```java
public interface Comparable<T> {
	public int compareTo(T obj);
}

public class Dog implements Comparable<Dog> {
	...
	public int compareTo(Dog uddaDog) {
		return this.size - uddaDog.size;
	}
}
```

对于对象的比较，Java 还提供了一种内置库 Comparator：
```java
import java.util.Comparator;

public class Dog {
	public String name;

	private static class NameComparator implements Comparator<Dog> {
		public int compare(Dog a, Dog b) {
			return a.name.compareTo(b.name);
		}
	}
	
	public static Comparator<Dog> getNameComparator() {
		return new NameComparator();
	}
}
```

```java
Comparator<Dog> nc = Dog.getNameComparator();
System.out.println(nc.compare(d1, d2));
```

本质上，Comparator 是一个 interface 类：
```java
public interface Comparator<T> {
	int compare(T o1, T o2);
}
```
类似这样⬆️。