#FullStackOpen 
## Same Origin Policy and CORS
如果我们尝试直接将本地后端和前端连接在一起，就会发现：![[Pasted image 20260411092505.png]]

这是因为 **同源策略**(Same Origin Policy) 限制了本地前后端之间的交互。

一个 URL 的源被定义为其协议、主机名和端口的组合：
```js
http://example.com:80/index.html
  
protocol: http
host: example.com
port: 80
```

当我们访问一个网页时，浏览器会向网站的宿主机发送请求，服务器响应一个 HTML 文件，该文件可能包含若干连向外部资源的引用，浏览器看到引用时，会向对应的 URL 继续发送请求。

如果该 URL 与 HTML 文件同源，那么浏览器就会直接正常处理该请求；但如果不同源，浏览器则会检查 `Access-Control-Allow-origin` 响应头。若响应头的值包含 `*` 或该 HTML 的 URL，那么浏览器会同意处理这次响应，否则就会拒绝并抛出错误。

这就被称为“同源策略”，是浏览器的一种安全机制。

而为了允许合法的跨源请求，需要使用 CORS （跨源资源共享，Cross-Origin Resource Sharing）机制。
> _跨源资源共享（CORS）是一种允许网页上的限制性资源（如字体）从所托管的域之外的域请求的机制。网页可以自由嵌入跨源图像、样式表、脚本、内联框架和视频。某些“跨域”请求，特别是 Ajax 请求，在默认情况下会被同源安全策略所禁止。_

我们的前端应用运行在 5173 端口，后端在 3001 端口，它们不属于相同的源，故它们无法直接通信。

不过，我们可以用 Node 的 `cors` 中间件来允许其他源的请求：
```js
const cors = require('cors')

app.use(cors())
```