继承是一种可以将类彼此联系起来的方式。

创建一个 base class 的子类：
```python
class <name>(<base class>):
	<suite>
```
子类可以覆写父类的属性，同时共享其他未被覆写的属性。

在子类中查找变量时，总是沿着继承链逐级向上。父类的属性并没有被复制入子类。

```python
isinstance(obj, class) # 判断obj是否为class的一个实例
```