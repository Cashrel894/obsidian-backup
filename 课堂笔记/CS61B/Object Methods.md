#CS61B 
## String toString ()
返回对象的字符串表示。实际上，`Systen.out.print()` 就调用了 `toString()` 方法。
```java
@Override
public String toString() {
	StringBuilder returnSB = new StringBuilder("{");
	for (int i = 0; i < size; i ++) {
		returnSB.append(items[i]);
		returnSB.append(", ");
	}
	returnSB.append(", ");
	return returnSB.toString();
}
```
由于在 Java 中，`String` 是 immutable 的，直接拼接效率较低，因此这里使用内置的 `StringBuilder` 构建返回的字符串。

## boolean equals (Object obj)
用于比较两个对象是否相等。和只比较两个引用是否相同的 `==` 不同，`equals` 可以用于检查对象的内容是否相同。
```java
@Override 
public boolean equals(Object o) {
	if (o == null) { return false; }
	if (this == o) { return true; }
	if (this.getClass() != o.getClass()) { return false; }
	ArraySet<T> other = (ArraySet<T>) o;
	if (this.size() != other.size()) { return false; }
	for (T item : this) {
		if (!others.contain(item)) {
			return false;
		}
	}
	return true;
}
```
