#FullStackOpen 
当我们加载 [https://studies.cs.helsinki.fi/exampleapp/]() 时，通过观察 Network 标签页的信息，我们发现有两个事件发生：
- 浏览器从服务器获取了页面的内容
- 并且下载了图片 `kuva.png`
![[../../附件/Pasted image 20250930170341.png]]

点击事件查看其详细信息，点击 `Headers`，发现请求和响应都有多个 header。![[../../附件/Pasted image 20250930170420.png]]

其中，一个比较重要的 header 是 `content-type`，它告诉浏览器返回文件的类型，这样浏览器才知道如何解析文件并呈现给用户。

点开 `Response` 标签，发现返回了一个 html 文件，其中包含一个 img 标签。浏览器解析文件后，由于 img 标签的存在，浏览器发送了第二次请求，来获取 kuva.png。![[../../附件/Pasted image 20250930171344.png]]