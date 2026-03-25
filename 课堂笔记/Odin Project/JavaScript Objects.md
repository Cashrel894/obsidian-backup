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

注意：默认情况下，函数的 `prototype` 总是会包含一个 `constructor` 属性，其值为函数本身。

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

可以用以下方法检验一个对象的原型：
```js
Array.prototype.isPrototypeOf(y);      // true
Object.prototype.isPrototypeOf(Array); // true

y instanceof Array; // true
```

那么怎么方便地进行原型继承呢？可以利用 `Function.prototype.call(thisarg, arg1, ...)` 方法：
```js
...
// Initialize Warrior constructor
function Warrior(name, level, weapon) {
  // Chain constructor with call
  Hero.call(this, name, level);

  // Add a new property
  this.weapon = weapon;
}

// Initialize Healer constructor
function Healer(name, level, spell) {
  Hero.call(this, name, level);

  this.spell = spell;
}
```

然而这还不够，因为子对象的原型不会自动链接到父对象的原型，因而无法访问父对象的方法。因此，需要使用 `setPrototypeOf(x, y)` 来设置原型：
```js
...
Object.setPrototypeOf(Warrior.prototype, Hero.prototype);
Object.setPrototypeOf(Healer.prototype, Hero.prototype);

// All other prototype methods added below
...
```

## The Value of `this`
**重要**：无论一个函数在原型链上的任何地方被找到，它所关联的 `this` 指针总是指向 `.` 之前的对象。

因此，虽然方法是共享的，但对象的状态不会。

## for ... in loop
`for ... in` 循环也会遍历继承的属性。例如：
```js
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys only returns own keys
alert(Object.keys(rabbit)); // jumps

// for..in loops over both own and inherited keys
for(let prop in rabbit) alert(prop); // jumps, then eats
```

当然，也可以用 `hasOwnProperty(key)` 方法来筛除那些继承的属性。

不过，有些属性描述符 (Property Descriptor) 中的 `enumerable` 为 `false`，就不能被 `for ... in` 列出。

几乎其他所有获取对象所有键/值的方法都会忽略继承的属性，如 `Object.keys`、`Object.values`。