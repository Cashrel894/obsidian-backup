#OdinProject 
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

