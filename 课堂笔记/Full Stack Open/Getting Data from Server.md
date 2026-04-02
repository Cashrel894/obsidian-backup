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

