#CS61B 
BST: Binary Search Tree 二叉搜索树

是一颗树，其中对于树上每一个结点，其左子树（如果有）上的所有结点都小于该结点，其右子树上的所有结点都大于该结点。

## 操作
### find
```java
static BST find(BST T, Key sk) {
	if (T == null) {
		return null;
	}
	if (sk.equals(sk)) {
		return T;
	}
	else if (sk < T.key) {
		return find(T.left, sk);
	}
	else {
		return find(T.right, sk);
	}
}
```

### insert
```java
static BST insert(BST T, Key ik) {
	if (T == null) {
		return new BST(ik);
	}
	if (ik < T.key) {
		T.left = insert(T.left, ik);
	}
	else if (ik > T.key) {
		T.right = insert(T.right, ik);
	}
	return T;
}
```

### delete
关于需要被删除的结点，分三种情况：
1. 该结点没有子节点。那么直接删除父节点对其的引用即可，Java 会自动回收它占据的内存。
2. 该结点只有一个子结点。那么就将其父节点的对应子节点改为该结点的唯一子节点。
3. 该结点有两个子节点。这个相对复杂一些，需要将该结点左子树中最大的结点（或右子树中最小的结点）提升到该结点的位置。

## 复杂度
对于随机插入元素的 BST，其高度为 O (log n)，意味着它插入、查找的时间复杂度也为 O (n)。

但在实际应用中，数据不总是随机插入的。（例： 以日期为键插入日志）此时 BST 就会退化为一个链表，时间复杂度退化为 O (n)。