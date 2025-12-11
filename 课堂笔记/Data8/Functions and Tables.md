#Data8 
```python
def cut_off_at_100(x):
    """The smaller of x and 100"""
    return min(x, 100)
    
ages = Table().with_columns(
    'Person', make_array('A', 'B', 'C', 'D', 'E', 'F'),
    'Age', make_array(17, 117, 52, 100, 6, 101)
)

ages.apply(cut_off_at_100, 'Age')
```