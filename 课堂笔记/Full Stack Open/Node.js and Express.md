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