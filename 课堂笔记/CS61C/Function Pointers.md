#CS61C 
```c
int (*fn) (void *, void *) = &foo;
*fn(x, y);
```
**函数指针**指的是一个存储函数起始地址的变量。

函数指针可以用于定义高阶函数之类的东西。
例：
```c
void mutate_map(int arr[], int n, int(*fp)(int)) {
	for (int i = 0; i < n; i ++) {
		arr[i] = (*fp)(arr[i]);
	}
}

int multiply2(int x) { return 2 * x; }
int multiply10(int x) { return 10* x; }

int main() {
	int arr[] = {3, 1, 4};
	int n = sizeof(arr) / sizeof(arr[0]);
	print_array(arr, n); // 3 1 4
	mutate_map(arr, n, &multiply2);
	print_array(arr, n); // 6 2 8
	mutate_map(arr, n, &multiply10); 
	print_array(arr, n); // 60 20 80
}
```