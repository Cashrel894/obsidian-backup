#CS61C 
我们可以利用 [[Function Pointers]] 来创建适用于所有数据类型的函数，称为**泛型函数**（**Generic Functions**）。

## Example: Generic Swap
错误做法：
```c
void swap(void *ptr1, void *ptr2) {
	void temp = ptr1; // Error due to the void type
	*ptr1 = *ptr2; // Error due to dereferencing void ptr
	*ptr2 = temp; // the same
}
```

正确做法：
```c
void swap(void *ptr1, void *ptr2, size_t nbytes) {
	char temp[nbytes];
	memcpy(temp, ptr1, nbytes);
	memcpy(ptr1, ptr2, nbytes);
	memcpy(ptr2, temp, nbytes);
}
```

`void *` 类型的指针不能直接做加减运算，可以先转换为 `char *` 类型，随后再按字节数进行计算。

