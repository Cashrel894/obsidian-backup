#DS100 
***数据清洗(Data Cleaning)***，又称***数据整理(Data Wrangling)***，是指**将原始数据转换为易于后续分析处理的形式**的过程，通常需要解决以下问题：
- 结构或格式不清晰
- 缺失值 / 错误值
- 单位转换

**探索性数据分析(EDA, Exploratory Data Analysis)**，是指理解一个新数据集的过程。通过 EDA，我们可以：
- 熟悉数据集中的各个变量
- 发现潜在的假设
- 识别数据可能的问题，从而进行进一步数据清洗

## Structure
通常而言，方形呈现的数据更加容易进行数据分析。方形数据包括**表格**和**矩阵**，前者可以用 pandas 等工具进行操作，后者则需要用到线性代数的变换方式。

有三种常见的数据文件格式：CSV、TSV 和 JSON。

### CSV
***CSV***，即 ***Comma-Separated Values***，用逗号和换行来分隔和组织数据。
```python
pd.read_csv("data/elections.csv")
```

### TSV
***TSV***，即 ***Tab-Separated Values***，与 CSV 唯一的不同点在于，它用缩进（即 `\t`）而非逗号来分隔同一行的数据。
```python
pd.read_csv("data/elections.txt", sep='\t')
```

### JSON
***JSON***，即 ***JavaScript Object Notation***。
```python
pd.read_json('data/elections.json')
```

## Variable Types
![[Pasted image 20260106211056.png]]

