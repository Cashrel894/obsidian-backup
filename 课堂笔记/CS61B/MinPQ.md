#CS61B 
```java
public interface MinPQ<Item> {
	public void add(Item x);
	public Item getSmallest();
	public Item removeSmallest();
	public int size();
}
```

假设我们要解决这样一个问题:
一段时间内接收 N 个 int，接收结束后返回最小的 M 个数。

方案 1:
使用 ArrayList 存储全部 N 个 int，接收完成后排序并返回。
空间复杂度：O (N)

方案 2:
使用 MinPQ (Minimum Priority Queue)，始终只保留最小的 M 个 int。
空间复杂度：O (M)

可见，方案 2 相比方案 1 在空间上具有一定优势，特别是当 M 远小于 N 时。