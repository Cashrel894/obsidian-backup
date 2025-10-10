#CS61B 
## Notation
```java
int[] x;
x = new int[3];
int[] y;
y = new int[]{1, 2, 3, 4, 5};
int[] z = {1, 2, 3, 4, 5};
```

## Modification
```java
System.arraycopy(b, 0, x, 3, 2);
```
五个参数分别表示：
- 源数组
- 源数组复制起始下标
- 目标数组
- 目标数组复制起始下标
- 复制长度
示例等价于 Python 中的 `x[3:5] = b[0:2]`

通常，arraycopy 的效率高于循环赋值，但可读性会大打折扣。

## 2D Arrays
```java
int[][] a = new int[4][];
```