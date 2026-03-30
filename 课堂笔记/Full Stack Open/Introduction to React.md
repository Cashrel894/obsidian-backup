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