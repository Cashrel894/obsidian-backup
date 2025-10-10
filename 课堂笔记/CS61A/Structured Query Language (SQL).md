## SELECT
一个 select 声明总是包含一系列由逗号分割的列陈述。

列陈述就是一个表达式，可以附加一个 AS \[name] 作为别名。
```sql
SELECT [expression] AS [name], [expression] AS [name];
```

### UNION
可以用 UNION 将多个 select 查询的结果合并为一张表。
```sql
SELECT ... UNION
SELECT ... UNION
SELECT ...;
```
这样得到的表将包含所有 select 语句返回的行。

注意：select 返回的结果只会显示给用户，而并不会存储下来。

### FROM & ORDER BY
```sql
SELECT [columns] FROM [tables] WHERE [conditions] ORDER BY [order];
```
FROM 语句可以接受多张表（用逗号分割），得到各表各列数据所有可能的组合作为行的表。

### Arithmetic
在 select 表达式中允许一些基本运算。
```sql
select a, b + 3 * c as res from some_table;
```
此外，where 语句的条件也支持 AND OR NOT 等基本逻辑运算。

### JOIN
除了在 from 语句中隐式地合并表，也可以用 join... on 显式合并。
```sql
SELECT ... FROM A JOIN B ON [some_conditions] ...
```
一般来说显式合并更加常见。

## CREATE TABLE
create table 则会将 select 的结果存储为一张表。
```sql
CREATE TABLE (IF NOT EXISTS) [name] AS [select statement];
```
或者，也可以创建一张新表，并指定各列的名称和约束条件。
```sql
CREATE TABLE [name] (IF NOT EXIST) ([column-def1], [column-def2], ...);
```
其中 column-def 由 column-name（必填） 和 column-constraint（选填） 组成，而最常见的 constraint 有 UNIQUE、DEFAULT 等。

## DROP TABLE
顾名思义，将指定的表删除。
```sql
DROP TABLE (IF EXISTS) [name];
```

## INSERT
将数据插入到表中。
```sql
INSERT INTO t(column) VALUES (val0), (val1), ...;
INSERT INTO t VALUES (val00, val01, ...), (val10, val11, ...), ...;	
INSERT INTO t SELECT ...;
```

## DELETE
删除指定的行。
```sql
DELETE FROM t (WHERE epxr);
```

## UPDATE
```sql
UPDATE t SET column0=expr0, column1=expr1, ... (WHERE expr);
```

## Aliases & Dot Expressions
有时两张表共享一个列名（或者就是同一张表），此时可以用 dot expression 区分两列。
```sql
SELECT a.i AS first, b.i AS second
	FROM T AS a, T AS b
	WHERE a.i = b.i AND a.j < b.j
```

## 数字表达式
SQL 的表达式与 Python 十分相似。
```sql
+ - * / % and or
abs round not -
< <= > >= <> != =
```

## 字符串表达式
```sql
sqlite> select "hello," || " world";
hello, world
```
substr (s, begin, len) 获取 s 的子串。
instr (s, t) 获取子串 t 第一次出现在 s 中时的下标。

## Aggregate Functions
这种函数可以获取整列的信息，如 MAX (), MIN (), AVG (), COUNT () 等。

其中，COUNT 函数有一个特殊的关键字：DISTINCT，可以去除重复数据后计数。

此外，部分函数可以选出整个行的数据，例如 `SELECT min(a), b FROM t` 会选出 a 最小的行及其对应的 a 和 b。

## GROUP
```sql
SELECT [columns] FROM [table] GROUP BY [group_expression] HAVING [having_expression];
```
group_expression 结果相同的行将被整合为一组，若在 columns 中使用 aggregate function，那么就会返回该函数在各个组上调用的结果，每组在新表中占据一行。

having 则可以筛选符合条件的组
