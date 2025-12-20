#Data8 
***A/B测试(A/B Testing)***：指检验两组数值样本是否来源于同一分布的测试。
***置换检验(Permutation Test)***：为了检验 A/B 测试的零假设（即检验统计量与样本组别无关），可以为所有个体重新随机置换所处组别后，计算并比较检验统计量。注意：这一过程中保证两组的大小不变，这保证了两个检验统计量的可比较性。

例：探究*母亲是否抽烟*与*婴儿出生体重*的关系中，比较的两组数据为“母亲抽烟”时的婴儿出生体重与“母亲不抽烟”时的婴儿出生体重。![[../../附件/Pasted image 20251220171819.png]]

在此案例中：
**零假设**：婴儿出生体重与母亲是否抽烟无关，即两组样本的差别只是出于偶然。
**替代假设**：婴儿出生体重与母亲是否抽烟相关，当母亲抽烟时，婴儿出生体重相对更小。
**检验统计量**：两组样本中，婴儿出生体重的平均值的差。

接着进行置换检验：
```python
shuffled_labels = smoking_and_birthweight.sample(with_replacement = False).column(0)
original_and_shuffled = smoking_and_birthweight.with_column('Shuffled Label', shuffled_labels)
```
![[../../附件/Pasted image 20251220180220.png]]

```python
def one_simulated_difference_of_means():
    """Returns: Difference between mean birthweights
    of babies of smokers and non-smokers after shuffling labels"""
    
    # array of shuffled labels
    shuffled_labels = births.sample(with_replacement=False).column('Maternal Smoker')
    
    # table of birth weights and shuffled labels
    shuffled_table = births.select('Birth Weight').with_column(
        'Shuffled Label', shuffled_labels)
    
    return difference_of_means(shuffled_table, 'Shuffled Label')   
```

```python
differences = make_array()

repetitions = 5000
for i in np.arange(repetitions):
    new_difference = one_simulated_difference_of_means()
    differences = np.append(differences, new_difference)
```

```python
Table().with_column('Difference Between Group Means', differences).hist()
print('Observed Difference:', observed_difference)
plots.title('Prediction Under the Null Hypothesis');
```
![[../../附件/Pasted image 20251220180431.png]]

结果的显著性水平极高，证明了替代假设是合理的。
