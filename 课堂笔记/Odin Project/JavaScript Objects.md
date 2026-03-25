#OdinProject 
## Methods
在 JS 中，对象不仅可以存储属性，还可以关联 **方法**，并通过 `this` 与对象进行交互：
```js
const car = {
  make: "Volkswagen",
  model: "Golf",
  year: 2026,
  color: "blue",
  priceUSD: 40000,

  // a method is just a function assigned to a property
  applyDiscount: function(discountPercentage) {
    const multiplier = 1 - discountPercentage / 100;
    this.priceUSD *= multiplier;
  },
  // shorthand way to add a method to an object literal
  getSummary() {
    return `${this.year} ${this.make} ${this.model} in ${this.color}, priced at $${this.priceUSD} (USD).`;
  },

  // ...any other methods...
};
```

## Constructors
可以通过构造函数来新建一个对象：
```js
function Player(name, marker) {
  this.name = name;
  this.marker = marker;
}

const player = new Player("steve", "X");
console.log(player.name); // "steve"
```

当我们通过 `new` 来调用函数时，它会：
1. 创建一个新对象；
2. 将函数上下文的 `this` 指向该对象；
3. 让该对象继承自函数的 `.prototype` 属性。
4. 执行函数体，最后隐式返回该对象。

### Safeguarding constructors
有时我们会忘记使用 `new` 调用构造函数，但解释器不会因此报错，因为 `this` 依然有其他指向，从而造成不可预料的后果。

因此，最好在构造函数第一行检查函数是否由 `new` 调用，并 Fail Fast：
```js
function Player(name, marker) {
  if (!new.target) {
    throw Error("You must use the 'new' operator to call the constructor");
  }
  this.name = name;
  this.marker = marker;
  this.sayName = function() {
    console.log(this.name);
  };
}
```

## The prototype
所有对象都有一个 **原型**(Prototype)，往往使用 `[[Prototype]]` 指涉，表示这是一个无法被直接访问的内部属性。对象总是继承自这个原型，并且可以访问该原型的所有属性和方法。

通过 `Object.getPrototypeOf(obj)` 或 `Obj.prototype`，我们就可以读取一个对象的原型。注意，`Obj` 指的是一个构造函数，而 `obj` 指的则是由 `Obj` 创建的一个实例。![[Pasted image 20260324171723.png]]

使用 `hasOwnProperty` 方法可以检查一个成员是对象本身的属性还是继承而来的：
```js
player1.hasOwnProperty("valueOf"); // false
Object.prototype.hasOwnProperty("valueOf"); // true
```

那么 `hasOwnProperty` 方法是从哪里来的呢？
```js
Object.prototype.hasOwnProperty("hasOwnProperty"); // true
```

