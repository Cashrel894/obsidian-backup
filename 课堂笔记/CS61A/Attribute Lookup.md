```python
<expression>.<name>
```
计算过程：
1. 先计算点左侧的 expression，得到一个对象。
2. 在该对象的 Attribute 中查找该 name，如果存在，就返回它的值。
3. 如果不存在，就在 Class 的 Attribute 中查找，如果得到一个函数，就作为与该对象绑定的方法返回；否则，直接返回这个值。

也可以用 getattr (obj, attr) 获取，好处是可以用 string 来查找属性。

hasattr (obj, attr) 则可以判断对象是否具有某个属性。