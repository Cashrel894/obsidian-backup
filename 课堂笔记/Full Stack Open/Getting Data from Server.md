#FullStackOpen 
## json-server
在开发调试前端应用时，使用 `json-server` 作为轻量的后端代理是个不错的选择：
```bash
npx json-server --port 3001 db.json
```

## Async way of JS
异步事件处理是 JS 的特色之一。例如：
```js
const xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    // handle the response that is saved in variable data
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```
在这个例子中，我们先为 `xhttp` 对象 *注册* 了一个事件处理器，直到事件发生才会被 JS 引擎调用，类似于操作系统中的中断机制。

## Promise
`Promise` 是一种表示异步操作的最终结果的对象。一个 `Promise` 有三种可能状态：
- `pending`：表示相应的异步操作还未结束，最终结果还无法访问。
- `fulfilled`：表示异步操作已经完成，可以访问最终结果。
- `rejected`：表示操作过程中发生了异常，操作失败。

要获取 `Promise` 的结果，就需要为其注册一个事件处理器：
```jsx
const promise = axios.get('http://localhost:3001/notes')

promise.then(response => {
  console.log(response)
})
```
其中，`response` 包含了所有 `HTTP GET` 请求返回的必要数据，包括返回的 `data`、`status code`、`headers` 等。

不过，更常见的写法是将请求和事件处理器链接在一起：
```jsx
axios
  .get('http://localhost:3001/notes')
  .then(response => {
    const notes = response.data
    console.log(notes)
  })
```
在这里，`axios` 库通过 `content-type` 元信息，识别到数据格式是 `application/json; charset=utf-8`，从而自动将返回的字符串数据解析为一个 JS array。

## Effect Hooks
除了 `state` 钩子之外，React 还引入了 `effect` 钩子。`effect` 钩子用于将组件连接并同步到外部系统，这包括处理网络、浏览器 DOM、动画、使用不同 UI 库编写的组件以及其他非 React 的代码。

