#CS61C
堆内存则是可以动态分配的内存，它们可以在程序运行时被手动分配、调整大小和释放等操作。
- 对于需要跨函数读写的数据很有用
- 然而非常容易导致内存泄露等问题

堆内存拥有远大于栈内存的空间，但其内存块并不是连续分配的。即使是连续的堆内存请求，分配的空间也可能相隔很远。

C 语言中管理内存的方式有：
- `void *malloc(size_t n)` 从堆内存中分配未初始化的内存，并返回指向这块内存的泛指针。如果没有多余的内存可以分配，将会返回**空指针**（使用时一定要检查空指针！）。
	- `typedef struct {...} TreeNode; TreeNode *tp = (TreeNode *) malloc(sizeof(TreeNode));` 新建结构体
	- `int *ptr = (int *) malloc(20 * sizeof(int)); if (ptr != NULL) {...}`
- `void free(void *ptr)` 从堆内存动态释放内存。其中 ptr 应是被 `malloc` 或 `realloc` 返回的指针，否则会导致程序崩溃亦或是更糟糕的后果。
	- `int *ptr = (int *) malloc (sizeof(int) * 20); ...; free(ptr);` 
- `void *realloc(void *ptr, size_t size);` 重新分配一块已分配内存的大小，并返回新内存块的地址。
	- realloc 时，程序可能需要将所有的数据复制到一个新地址。
	- `realloc(NULL, size); // 和malloc差不多`
	- `realloc(ptr, 0); // 和free差不多`
	- `int *ip; ip = (int *) malloc(10 * sizeof(int)); ip = (int *) realloc(ip, 20 * sizeof(int)); realloc(ip, 0);`
