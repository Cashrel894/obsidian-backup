#FullStackOpen 
React 的核心思想是：将网页的组成部分抽象为一个个可复用的 **组件**(Component)。

基础使用非常简单，例如：
```jsx
const Hello = (props) => {
  console.log(props);
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  );
};

const App = () => {
  const name = 'Peter';
  const age = 10;

  return (
    <div>
      <h1>Greetings</h1>

      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  );
};
```

注意：
- React 中的所有组件名称都应该**首字母大写**，否则可能被识别为原生 html 标签。
- 每个 React 组件都应该有一个 **根元素**，例如：
```jsx
const App = () => {
  return (
    <h1>Greetings</h1>
    <Hello name='Maya' age={26 + 10} />
    <Footer />
  )
}
```
这样会报错。

解决方案：
```jsx
const App = () => {
  return [
    <h1>Greetings</h1>,
    <Hello name='Maya' age={26 + 10} />,
    <Footer />
  ]
}
```
或者更常用的是用一个空标签包裹：
```jsx
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}
```
- React 不能直接渲染 JS 对象，只能直接渲染原生数据类型，例如：
```jsx
const App = () => {
  const friends = [
    { name: 'Peter', age: 4 },
    { name: 'Maya', age: 10 },
  ]

  return (
    <div>
      <p>{friends[0]}</p>
      <p>{friends[1]}</p>
    </div>
  )
}

export default App
```
无法正常渲染。
- React 支持直接渲染 Array。

## Stateful component
一个组件的状态可能随着用户交互等发生变化，当状态更新时，要自动重渲染这些状态，就需要使用 React 的 **状态 hook**（State hook）。
```jsx
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}

export default App
```

`useState(x)` 为组件添加了状态，并将其初始化为 `x`。该函数返回包含两个项目的数组，第一个 `counter` 为 *状态初始值*，第二个 `setCounter` 则为 *修改状态的函数*。

每当 *修改状态的函数* 被调用时，React 会重新渲染组件，即重新执行组件的函数体。此时，`counter` 会被赋予更新后的状态值，接着调用 `setCounter`，渲染组件；如此循环往复。

