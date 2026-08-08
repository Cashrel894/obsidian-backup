#ctf/web 
[[Injection]] 主要是客户端对服务器的攻击，而跨站脚本攻击(Cross Site Scripting, XSS)则主要是对同一服务端的其他客户端的攻击，通过在返回的 HTML 页面中内嵌恶意脚本来控制客户端浏览器、从而发动攻击。

## Stored XSS
存储型 XSS，是指用户提交的恶意脚本会在数据库存储后发送给其他客户端，从而达成攻击者的目的。

## Reflective XSS
反射型 XSS 则略有不同，这要求受害者必须主动访问一个带有特殊参数的 url，从而触发脚本执行攻击。

## Cross-Site Request Forgery