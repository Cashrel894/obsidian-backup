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

