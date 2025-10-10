#OdinProject 
**事件**除了被 [[Mouse Events]] 和 [[Keyboard Events]] 生成之外，还可以由 JS 代码自行创建。

## Event Constructor 
```js
let event = new Event(type, [,options]);
```
Event 构造函数接受两个参数：
- type：字符串，表示事件的类型，如 `"click"`
- options：接受一个对象，包含两个属性：
	- `bubbles`：布尔值，表示事件是否会冒泡。
	- `cancelable`：布尔值，表示事件能否被取消。（如使用 `preventDefault()`）
	- 默认情况下，上述两个值都是 false

## dispatchEvent method 
```js 
element.dispatchEvent(event);
```

> [!note] 
> 如果一个事件由用户操作生成，它的 `isTrusted` 属性值为 true；否则，若由代码直接生成，则为 false。我们可以通过检查 `isTrusted` 属性值来一定程度上防范脚本。
