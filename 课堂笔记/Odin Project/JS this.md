#OdinProject 
在 js 中，`this` 关键字用于指涉拥有当前执行的函数或方法的对象。在全局上下文中，`this` 指涉的是 **全局对象**，在浏览器中是 `window`，在 Node. js 中是 `global` 对象。

## Function Context
`this` 的值通常取决于你如何调用一个函数。有四种主要方式：
1. 简单函数调用：在非严格模式下，会指向全局对象；在严格模式下，则为 `undefined`。
2. 方法调用：指向调用该方法的对象。实际上，通过函数的 `Function.prototype.bind(obj)` 方法，可以将函数“伪装”成方法，简单函数调用相当于在该对象上的方法调用。
3. 构造函数调用：调用构造函数时，js 会新建一个对象，并将 `this` 设为这个新对象。为避免构造函数被简单调用，可以使用 `if(!new.target) throw ...` 检查。
4. 间接调用：在 js 中，函数是一等公民，也属于对象的范畴。一个函数有两个方法：`call` 和 `apply`。`call` 和 `apply` 的第一个参数指定了函数调用的 `this` 指向；而函数调用的参数为 `call` 的剩余参数，或 `apply` 的第二个参数的所有元素（第二个参数为一个 `Array`）。

## Array Functions
箭头函数与普通函数最大的不同正出在对 `this` 的处理上。

箭头函数不会创建自己的执行上下文，而是会直接从外层函数继承 `this`。
```js
let getThis = () => this;
console.log(getThis() === window); // true
```

