#OdinProject 
## Create Empty Objects 
```js
let user = new Object();
let user = {};
```

## Delete Object Property 
```js
delete user.age;
```

## Multiword Property Name 
```js
let user = {
	name: "John",
	age: 30,
	"likes birds": true,
}
```

```js
user["likes birds"] = false;
```

```js
let key = "likes birds";

console.log(user[key]); // OK!
console.log(user.key);  // WRONG!
```

## Computed Properties 
```js
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
	[fruit]: 5,
}

alert(bag.apple); // 5 (if fruit === "apple")
```

等价于：
```js
// fruit omitted
let bag = {};
bag[fruit] = 5;
```

方括号内可以是一个表达式：
```js
let bag = {
	[fruit + "Computers"]: 5,
}
```

## Property Value Shorthand
当属性名与所要赋值给属性的变量名相同时，可以使用以下简写形式：
```js
function makeUser(name, age) {
	return {
		name, // name: name,
		age,  // age: age,
		salary: 3000, // 允许混用
	}
}
```