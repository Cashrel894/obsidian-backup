## Raise Statements
```python
raise <expression>
```
expression 的计算结果必须是 BaseException 的一个子类（或子类的实例）
例：
```python
raise TypeError('Bad argument!')
```
![[../../附件/Pasted image 20250811132106.png]]

## Try Statements
```python
try:
	<try suite>
except <exception class> as <name>:
	<except suite>
```
1. 首先执行 try suite
2. 如果在执行过程中，有没被处理的 exception 产生，且该 exception 继承自 exception class（允许接受一个元组），那么该 exception 被绑定到 name，并执行 except suite