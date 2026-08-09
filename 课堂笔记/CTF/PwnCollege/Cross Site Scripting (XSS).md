#ctf/web 
[[Injection]] 主要是客户端对服务器的攻击，而跨站脚本攻击(Cross Site Scripting, XSS)则主要是对同一服务端的其他客户端的攻击，通过在返回的 HTML 页面中内嵌恶意脚本来控制客户端浏览器、从而发动攻击。

## Stored XSS
存储型 XSS，是指用户提交的恶意脚本会在数据库存储后发送给其他客户端，从而达成攻击者的目的。

## Reflective XSS
反射型 XSS 则略有不同，这要求受害者必须主动访问一个带有特殊参数的 url，从而触发脚本执行攻击。

## Cross-Site Request Forgery (CSRF)
### Same-Origin Policy (SOP)
在了解 CSRF 之前，需要先了解什么是同源策略。

HTTP URL 的结构如下：
```plain
<scheme>://<host>:<port>/<path>?<query>#<fragment>
```

其中，源(Origin)包含 scheme、host 和 port 三部分，可以表示为：
```plain
(<scheme>, <host>, <port>)
```
两个 url同源当且仅当它们的 scheme、host 和 port 都相等，否则不同源。

而同源策略，则是对跨源请求和响应类型的限制。

具体来说，跨源请求只允许简单的请求：
- 只允许 `GET`、`HEAD` 和 `POST` 方法
- 只允许以下 Header：
	- `Accept`
	- `Accept-Language`
	- `Content-Language`
	- `Content-Type`，只接受以下值：
		- `application/x-www-form-urlencoded`
		- `multipart/form-data`
		- `text/plain`
	- `Range`（仅接受简单数值）

而跨源请求只允许可内嵌在 HTML 的元素：
- Images: `<image>`
- Media: `<video>` 和 `<audio>`
- External Resources: `<object>` 和 `<embed>`
- Inline Frames: `<iframe>`
- CSS: `<link rel="stylesheet" href="...">`
- JavaScript: `<script src="..."></script>`

### Cookie Attribute
域名(Domain)：由多个 `.` 分割的标签组成
顶级域名(Top Domain)：最右侧的标签 
有效顶级域名(Effective Top Domain)：通常包含 1~2 个最右侧标签，具体可参见 [https://publicsuffix.org/list/public_suffix_list.dat]
网站(Site)：有效顶级域名+左侧一个标签
![[Pasted image 20260808191332.png]]

Cookie 可以指定多种属性：

#### SameSite
- `SameSite=None`：Cookie 允许在跨站请求中附带。
- `SameSite=Lax`：默认选项，Cookie 只允许在顶级导航 GET （即用户输入网址或点击链接的 GET 请求）中附带。
- `SameSite=Strict`：Cookie 不在跨站请求中附带。

#### Domain
指定允许附带 Cookie 的请求目标域名（及其任意子域名）。

若未指定，那么只有 `Host` 域名允许附带 Cookie。

#### Path
指定允许附带 Cookie 的请求路径（及其任意子路径）

#### httponly
当设置了 `httponly` 属性时，Cookie 只能在 HTTP 请求头中被访问。这也就意味着无法通过 JS 访问 Cookie。

### Cross-Origin Resource Sharing (CORS)
跨源资源共享是浏览器提供的一种允许或限制跨源 HTTP 请求的机制。

简单来说：
> 浏览器默认禁止网页向不同源（origin）的服务器发送/读取数据，CORS 通过 HTTP 响应头告诉浏览器哪些跨源请求可以被允许。

在发送复杂请求之前，会先发送 **预检请求**(Preflight requests)：
```http
OPTIONS /api
```
附带以下请求头：
```http
Origin: https://app.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-token
```
服务器响应批准：
```http
HTTP/1.1 204 No Content

Access-Control-Allow-Origin: https://app.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: X-token
```
然后浏览器才会真正发送请求。