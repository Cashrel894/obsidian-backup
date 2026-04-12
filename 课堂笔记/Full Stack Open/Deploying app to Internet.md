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

