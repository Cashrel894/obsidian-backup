#Data8 
***均方根误差 (RMSE, Root Mean Squared Error)***：常用于衡量拟合的精确程度。最小二乘法就是以最小化 RMSE 为目标。而**回归直线**正是符合预期的那条直线。这也就是为什么回归直线也被称作“**最小二乘直线**”。

使用 `datascience` 库，我们可以自动拟合使得指定函数返回值最小的参数组合：
```python
minimize(function)
```

