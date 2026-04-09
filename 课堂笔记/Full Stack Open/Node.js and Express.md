#FullStackOpen 
## Simplest web server
```js
const http = require('http')

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})

const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

其中，`require('http')` 来自 CommonJS 模块系统，效果类似于 `import http from 'http'`。

`http.createServer` 创建了一个 web 服务器，并绑定了一个 *事件处理器*，在服务器每次收到 HTTP 请求时调用。

这里，服务器将状态码 200 和 Content-Type 写入响应头，并以 `Hello World` 作为响应体返回到客户端。

最后让服务器监听发送到 3001 端口的 HTTP 请求。

## Express
Node 内置的 http 服务器足以实现一个服务器，但有时过于笨重、难以扩大规模。Express 是一个流行的后端开发框架，可以大大简化服务器开发流程。

```js
const express = require('express')
const app = express()

let notes = [
  ...
]

app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})

app.get('/api/notes', (request, response) => {
  response.json(notes)
})

const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

在这段代码中，我们首先导入 `express` 构造函数，随即用其创建一个服务器对象 `app`。

接着，我们定义了两个路由 (route)，第一个用于处理到根路径的路由。回调函数中，`request` 参数包含 HTTP 请求的所有的所有信息，`response` 参数则用于定义响应请求的信息。

