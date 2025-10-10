#CS61B 
泛型
允许类接受任意类型的数据。

用法示例：
```java
public class DLList<BleepBlorp> {
    private IntNode sentinel;
    private int size;

    public class IntNode {
        public IntNode prev;
        public BleepBlorp item;
        public IntNode next;
        ...
    }
    ...
}
```

```java
DLList<String> d2 = new DLList<>("hello");
```

> [!warning] 
> 泛型只能使用 [[Reference Types]]。如果要使用 [[Primitive Types]]，可以考虑引用类型版本的替代品，如 `Interger` `Double` `Character` `Boolean` `Long` `Short` `Byte` `Float` 等。

> [!note] 
> `DLList<Integer> d = new DLList<>(5)` 
> 也可以写成 
> `DLList<Integer> d = new DLList<Interger>(5)`
>  后面一个 `Integer` 可以省略，不会影响编译，但前面一个不能。

非静态的内部类可以直接访问外部类的泛型，而由于泛型与实例本身指定的类型有关，静态内部类是无法访问的。