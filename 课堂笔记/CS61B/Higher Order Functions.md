#CS61B 
在 Java 中，可以利用 [[interface & implements]] 实现与 Python 类似的高阶函数。
```java
public interface IntUnaryFunction {
	int apply(int x);
}

public class TenX implements IntUnaryFunction {
	public int apply(int x) {
		return 10 * x;
	}
}

...
	public static int do_twice(IntUnaryFunciton f, int x) {
		return f.apply(f.apply());
	}
...
```