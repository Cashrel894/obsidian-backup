#ctf/web 
## 创建表
```sql
CREATE TABLE <table> (<columns>) 
```

##  插入行
```sql
INSERT INTO <table> VALUES (<values>)
```

## 查询数据
```sql
SELECT <columns> FROM <table> WHERE <conditions>
```

## 删除行
```sql
DELETE FROM <table> WHERE <conditions>
```

## 更新行
```sql
UPDATE <table> SET <assignments> WHERE <conditions>
```

## 联合查询
```sql
<select> UNION <select>
```
![[Pasted image 20260808095435.png]]

## 元数据
```sql
SELECT tbl_name FROM sqlite_master
```
![[Pasted image 20260808095514.png]]

## 删除表
```sql
DROP TABLE <table>
```