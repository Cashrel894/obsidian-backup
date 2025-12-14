#Data8 
[[Simulation]]

```python
two_groups = make_array('treatment', 'control')
np.random.choice(two_groups)
```
```
'treatment'
```

```python
np.random.choice(two_groups, 10)
```
```
array(['control', 'control', 'treatment', 'treatment', 'control', 'control', 'control', 'control', 'control', 'control'], dtype='<U9')
```

```python
tosses = make_array('Tails', 'Heads', 'Tails', 'Heads', 'Heads')
tosses == 'Heads'
```
```
array([False, True, False, True, True])
```

```python
np.count_nonzero(tosses == 'Heads')
```
```
3
```

```python
pets = make_array('Cat', 'Dog')
np.append(pets, 'Another Pet')
```
```
array(['Cat', 'Dog', 'Another Pet'], dtype='<U11')
```

