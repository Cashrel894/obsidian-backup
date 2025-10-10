#OdinProject 
DOM: Document Object Model
[[Mouse Events]]
[[Keyboard Events]]
[[dispatchEvent]]
[[customEvent]]

## DOM Methods
### Query Selectors
```html
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
```

```js
const container = document.querySelector("#container");

const display = container.firstElementChild;
console.log(display); // <div class="display"></div>
```

```js
const controls = document.querySelector(".controls");

const display = controls.previousElementSibling;
console.log(display);
```

- `element.querySelector(selector)`：获取 element 子节点中第一个符合 `selector` 的结点。
- `element.querySelectorAll(selector)`：获取 element 子节点中所有符合 `selector` 的结点组成的 `NodeList`

> [!warning] 
> `NodeList` 和 `Array` 存在差异，比如前者不支持很多后者的方法。
> 可以用 `Array.from()` 或 `...` 运算符将 `NodeList` 转换为 `Array`。

### Element Creation 
- `document.createElement(tagName, [options])`
	- 例：`const div = document.createElement("div");`

### Append Elements
- `parentNode.appendChild(childNode)`
- `parentNode.insertBefore(newNode, referenceNode)`

### Remove Elements
- `parentNode.removeChild(child)`

### Altering Elements
```js 
const div = document.createElement("div");

div.style.color = "blue";

div.style.cssText = "color: blue; background: white;";

div.setAttribute("style", "color: blue; background: white;");
```

```js
div.style.backgroundColor // OK!
div.style.background-color // WRONG! The '-' is seen as 'substract' here.
div.style["background-color"] // OK!
div.style["backgroundColor"] // also OK!
```

### Editing Attributes
```js
div.setAttribute("id", "theDiv");

div.getAttribute("id");

div.removeAttribute("id");
```

### Working with Classes
```js
div.classList.add("new");

div.classList.remove("new");

div.classList.toggle("active");
```

### Adding Text Content 
```js
div.textContent = "Hello World!";

div.innerHTML = "<span>Hello World!</span>";
```

> [!note] 
> JS 操作的是 DOM，不是 HTML。因此，JS 操作的是网页实际渲染的内容，而不是 HTML 文件本身。

> [!caution] 
> 在 HTML 中链接 js 文件时，由于 DOM 是顺序加载的，我们需要在所有 DOM 加载完成后再链接 js，否则 js 可能无法访问一些 DOM 结点。
> 解决方案是，可以将 `<script>` 放在 HTML 文件末尾，或者在 `<head>` 中放置 `<script>`，并加上 `defer` 属性，以在所有 HTML 解析完成后再加载 js。
> - 例：`<script src="script.js" defer></script>`

## Events
**事件**可以响应网页上的各种操作，让网页真正变得有互动性起来。

我们使用 JavaScript 有三种方式让网页监听事件：
- 直接在 HTML 中指定元素的 `on<event-type>` 属性。如 `<button onclick="alert("Hello World")">Click me</button>`
- 在 JS 中设置 `on<event-type>` 属性。
- 使用元素的 `addEventListener` 方法。

如果我们向回调函数添加参数，那么函数就能访问更多的信息。这个参数就是一个指向**事件本身**的对象，其中包含着很多关于事件有用的属性和方法：
- `e.target`：触发事件的元素本身。
- `e.key`：`keydown` 事件中用户按下的按键。
- 更多详见 https://developer.mozilla.org/en-US/docs/Web/API/Event

常用的有：

| Property / Method | Description                                                                                                     |
| ----------------- | --------------------------------------------------------------------------------------------------------------- |
| bubbles           | true if the event bubbles                                                                                       |
| cancelable        | true if the default behavior of the event can be canceled                                                       |
| currentTarget     | the current element on which the event is firing                                                                |
| defaultPrevented  | return true if the preventDefault() has been called.                                                            |
| detail            | more information about the event                                                                                |
| eventPhase        | 1 for capturing phase, 2 for target, 3 for bubbling                                                             |
| preventDefault()  | cancel the default behavior for the event. This method is only effective if the `cancelable` property is true   |
| stopPropagation() | cancel any further event capturing or bubbling. This method only can be used if the `bubbles` property is true. |
| target            | the target element of the event                                                                                 |
| type              | the type of event that was fired                                                                                |

事件也有很多类型，比如：
- `click`
- `dblclick`
- `keydown`
- `keyup`
- 更多详见 https://www.w3schools.com/jsref/dom_obj_event.asp

### Event Bubbling Model
在**事件冒泡模型**中，当事件发生时，事件会从触发事件的元素开始，逐级**向上浮动**，直到最高级的 `document`（或者现代浏览器中的 `window`）为止。

### Event Capturing Model 
在**事件捕获模型**中，事件移动顺序与冒泡模型相反，从根结点开始向下移动到触发事件的元素。


实际上，以上两个模型是整个事件传播过程中的两个阶段。事件的传播过程由 DOM Level 2 Events 定义：
1. **捕获阶段**：事件从根节点传播到目标结点
2. **目标阶段**：事件到达目标结点
3. **冒泡阶段**：事件从目标节点再传播到根节点
![[../../附件/Pasted image 20251008160653.png]]

事件监听器可以选择在哪个阶段捕获事件。

### preventDefault()
作为事件的方法，它可以阻止事件的默认行为发生。例如：
```html
<a href="https://example.com">Click me</a>
```
当用户点击这个 a 元素时，浏览器会导航到新的页面上去。

```js
let link = document.querySelector("a");

link.addEventListener('click', function(e) => {
	console.log('clicked');
	e.preventDefault();
});
```
若事件被调用了 `preventDefault()` 方法，就会阻止导航行为的发生。

### stopPropagation()
当该方法被调用时，会阻止该事件后续在 DOM Level 2 模型中的传播。