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

### Variable Types
![[Pasted image 20260106211056.png]]

## Granularity, Temporality and Faithfulness
> The **granularity** of a dataset is what a single row represents. You can also think of it as the level of detail included in the data. To determine the data’s granularity, ask: what does each row in the dataset represent? **Fine-grained data contains a high level of detail**, with a single row representing a small individual unit. For example, each record may represent one person. **Coarse-grained data is encoded such that a single row represents a large individual unit** – for example, each record may represent a group of people.

> The **temporality** of a dataset describes the periodicity over which the data was collected as well as when the data was most recently collected or updated.
> Time and date fields of a dataset could represent a few things:
> 1. **when the “event” happened**
> 2. **when the data was collected, or when it was entered into the system**
> 3. **when the data was copied into the database**

> Data used in research or industry is often “messy” – there may be errors or inaccuracies that impact the **faithfulness** of the dataset. Signs that data may not be faithful include:
> - **Unrealistic** or “incorrect” values, such as negative counts, locations that don’t exist, or dates set in the future
> - **Violations of obvious dependencies**, like an age that does not match a birthday
> - Clear signs that data was entered by hand, which can lead to **spelling errors** or fields that are incorrectly shifted
> - Signs of **data falsification**, such as fake email addresses or repeated use of the same names
> - **Duplicated** records or fields containing the same information
> - **Truncated** data, e.g. Microsoft Excel would limit the number of rows to 655536 and the number of columns to 255

### Missing Data
当出现数据缺失时，有多种解决方案：
1. 直接丢弃。
2. 保留为 `NaN` 值。
3. 执行**数据插补** (**imputation**)，即利用其他数据推断缺失值。

常用的数据插补方法：
1. **平均值插补**：顾名思义
2. **Hot Deck 插补**：用全体数据中与当前对象最相似的对象填补
3. **回归填补**：建立回归模型，用预测值填补
4. **多重填补**：使用多种填补方式分别生成填补值，然后综合归纳

当然最重要的**不是如何填补缺失值**，**而是思考值之所以缺失的原因**，这样才能帮助我们决定这些缺失值有没有必要填补、是否有重要意义。

## EDA and Data Wrangling
There are several ways to approach EDA and Data Wrangling:
- Examine the **data and metadata**: what is the date, size, organization, and structure of the data?
- Examine each **field/attribute/dimension** individually.
- Examine pairs of related dimensions (e.g. breaking down grades by major).
- Along the way, we can:
    - **Visualize** or summarize the data.
    - **Validate assumptions** about data and its collection process. Pay particular attention to when the data was collected.
    - Identify and **address anomalies**.
    - Apply data transformations and corrections (we’ll cover this in the upcoming lecture).
    - **Record everything you do!** Developing in Jupyter Notebook promotes _reproducibility_ of your own work!