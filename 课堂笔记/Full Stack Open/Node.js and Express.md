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

在 `res.send` 中，express 自动将 `Content-Type` 设为 `text/html`，并将状态码设为 200，最后自动调用 `end` 方法发送响应。

而 `res.json` 类似，只不过将 `Content-Type` 设为 `application/json`，并调用 `JSON.stringify` 序列化传入的对象。

## Restful API
![[Pasted image 20260409104134.png]]

## Fetching a single resource
在路由中，我们可以定义 *参数*：
```js
app.get('/api/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id)
  response.json(note)
})
```
这里，我们用 `:xxx` 来表示路由中的参数，并可以通过 ` request.params.xxx ` 来访问特定的参数。

然而这段代码有一定问题，如果我们访问了不存在的资源，`res.json` 就会被传入 `undefined`，进而响应一个 `Content-Length` 为 0 、响应体为空的成功响应。而我们应该响应的是 `404 not found`。

```js
app.get('/api/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id)
  
  if (note) {
    response.json(note)
  } else {
    response.status(404).end()
  }
})
```

附：如果要覆盖 express 默认的状态信息，可以利用原生的 http 库：
```js
// Source - https://stackoverflow.com/a/36507614
// Posted by mamacdon
// Retrieved 2026-04-09, License - CC BY-SA 3.0

function(req, res) {
    res.statusMessage = "Current password does not match";
    res.status(400).end();
}
```

## Delete
```js
app.delete('/api/notes/:id', (request, response) => {
  const id = request.params.id
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
```
通常，`delete` 请求的响应状态码为 `204 no content`，响应体为空。有时也可能返回 `404 not found`。

## Receiving data
要创建新的资源，前端需要向后端发送 HTTP POST 请求，并将新资源的相关信息用 JSON 格式存储在请求体中。

后端想要解析发来的 JSON，可以使用 Express 的 json-parser 中间件：
```js
const express = require('express')
const app = express()

app.use(express.json())

//...

app.post('/api/notes', (request, response) => {
  const note = request.body
  console.log(note)
  response.json(note)
})
```

在这里，在路由处理请求之前，json-parser 就先使用请求的 JSON 数据，将其转换为 JS 对象，并赋值给 req 对象的 body 属性。

## About HTTP request types
HTTP 请求类型需要满足一定的要求，例如在 HTTP 标准中：
- HTTP GET 和 HEAD 请求应当是 **安全的**。即，GET 只应该从服务器获取数据，不能有任何其他更改。
- 所有除 HTTP POST 之外的请求都应当是 **幂等的**。即，任意 $\displaystyle N>0$ 个相同的请求等效于单个请求。GET、HEAD、PUT、DELETE 都应当满足此要求。
可以发现，HTTP POST 请求既不安全，也不幂等。

## Middleware
中间件是一种用于处理 `request` 和 `response` 对象的函数。

先前的 json-parser 属于一种 Express **中间件**(Middleware)，它从 `request` 对象中提取出原始数据，接着将其解析为 JS 对象，最后赋值给 `request` 对象的 `body` 属性。

中间件可以有很多个，会依照代码顺序一个个执行。
```js
const requestLogger = (request, response, next) => {
  console.log('Method:', request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}
```
中间件接受三个参数：`request`、`response` 和 `next`。其中，`next` 被传入了下一个中间件，当前中间件可以控制下一中间件的执行。

通过 `app.use` 插入中间件：
```js
app.use(requestLogger)
```

如果我们想要让路由事件处理器自动执行中间件，那么中间件应当在路由定义之前被插入。但有时，我们会在路由定义之后插入中间件，用于处理任何未被路由处理的请求。例如：
```js
const unknownEndpoint = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

app.use(unknownEndpoint)
```

