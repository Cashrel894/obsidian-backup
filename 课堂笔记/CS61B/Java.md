#CS61B 
## 基本特性
Java 与 Python 对比：
1. Java所有变量在使用之前必须先声明，并指定其类型。
2. 在运行之前检查变量类型，而不是在运行中。
3. 所有函数都是一个类中的方法，且需要声明函数返回值和形参的类型。
好处：增强代码可读性，避免运行时出现类型错误。

## 编译
javac 指令编译 java 源代码（. java 文件），java 指令则运行编译产生的 .class 文件。

## 类 && 实例
### 基本定义
```java
public class Dog {
	public int weightInPounds; //An attribute
	
	public Dog(int startingWeight) { //Constructor in Java
		weightInPounds = startingWeight;
	}
	
	public void makeNoise() {
		System.out.println("bark!");
	}
}
```

```Java
Dog d = new Dog(15); //creates a Dog instance weighing 15 pounds
```

### static
static 方法和 non-static 方法（即实例方法）的区别：
- 前者需要用类名调用，如 `Dog.makeNoise()`
- 后者则需要通过实例调用，如 `doggy.makeNoise()`
- 前者无法访问实例的属性。
注意：静态上下文中不能使用非静态成员。

## 成员
绑定到一个类的变量、方法和内部类统称为**成员**。

## 列表
```java 
int[] a = new int[3];
a[0] = 1;
```

```java 
Dog[] dogs = new Dog[2];
dogs[0] = new Dog(8);
dogs[1] = new Dog(20);
```


