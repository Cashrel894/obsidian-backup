#Data8 
```python
cones.group('Flavor') # 等价于cones.group('Flavor', len)
```
![[../../附件/Pasted image 20251212194639.png]]

```python
cones.group('Flavor', sum) # 除了分类列外的所有列，都会被apply上collect函数
```
![[../../附件/Pasted image 20251212194731.png]]

```python
nba.group('POSITION', np.mean)
```
![[../../附件/Pasted image 20251212195205.png]]