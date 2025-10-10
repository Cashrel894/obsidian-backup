#CS61B 
> [!note] 
> 测试的目的：
> - 确保程序稳定性，帮助程序员专注于一件工作上。
> - 方便进行重构，以提高代码的可读性和运行效率。 
## JUnit
```java 
org.junit.Assert.assertArrayEquals(expected, actual);
```
除此之外，还有 `assertFalse` `assertNotNull` 等。

可以用 `@org.junit.Test` 标记测试函数来自动运行它们，而无需 main 函数。
> [!warning] 
> 所有测试方法必须是 non-static 的，因为 JUnit 会创建实例来运行它们。

也可以导入这些库，方便调用这些测试函数。
```java 
import org.junit.Test;
import static org.junit.Assert.*;

@Test 
public void testFunc() {
	...
	assertEquals(exprected, actual);
	...
}
```

## Testing Tools
### Unit Tests
![[../../附件/Pasted image 20250820110617.png]]

## Intergration Testing
![[../../附件/Pasted image 20250820110639.png]]

## Testing Workflows
### Test-Driven Development (TDD)
在编写代码前预先编写单元测试，不断迭代代码直到测试通过，随后可以在确保测试通过的前提下重构代码。
![[../../附件/Pasted image 20250820110146.png]]
但是这会让编程过于 Tricky，因此对于测试需要做好取舍和平衡。