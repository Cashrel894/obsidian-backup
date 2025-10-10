Class 描述其实例的共同行为（Attributes 和 Methods）。

```python
class Account:
	def __init__(self, account_holder): # __init__是一个特殊的函数名，用于实例的构建
		self.balance = 0
		self.holder = account_holder
	
	def deposit(self, amount): # self指向调用该方法的对象
		self.balance = self.balance + amount
		return self.balance
	def withdraw(self, amount):
		if amount > self.balance:
			return 'Insufficient funds'
		self.balance = self.balance - amount
		return self.balance
```