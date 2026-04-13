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

## Frontend Production Build
目前，我们都以开发模式运行 React 代码。要让应用部署到互联网上，就需要构建一个生产版本。

使用 Vite，只需要 `npm run build` ，即可创建一个叫作 `dist` 的目录，只包含应用的 HTML 文件及相关资源。所有 JS 代码也被压缩到了单个文件，被称为 **极简** 版本。

## Serving static files from the backend
一个前端部署策略是，将前端的生产版本复制到后端应用的根目录下，再配置后端使其主页面直接显示为前端的主页面（`dist/index.html`）。

接着，使用 Express 内置的 `static` 中间件：
```js
app.use(express.static('dist'))
```

每当 Express 收到 HTTP GET 请求时，它就会首先检查 dist 目录是否包含对应请求路径的文件。如果有，Express 就会返回它。

此时，对 `www.serversaddress.com/index.html` 或 `www.serversaddress.com` 的请求都将响应 `dist/index.html`，而对 `/api/notes` 的请求则会被后端正常处理。

用这种方式部署时，前后端都在相同的地址，因此可以直接用相对路径进行通信：
```js
import axios from 'axios'
const baseUrl = '/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```
在开发环境，我们的应用是这样的：![[Pasted image 20260413211851.png]]

而在生产环境，浏览器、前端、后端的关系如图所示：
![[Pasted image 20260413211804.png]]

## Streamlining deploying of the frontend
在后端的 `package.json` 中加入以下命令，可以自动完成前端构建和后端部署的过程。如果使用 Render 部署应用，那么每次 commit 到 Github 仓库时应用都会自动重新部署，非常方便。
```json
{
  "scripts": {
    //...
    "build:ui": "rm -rf dist && cd ../frontend && npm run build && cp -r dist ../backend",
    "deploy:full": "npm run build:ui && git add . && git commit -m uibuild && git push"
  }
}
```

## Proxy
由于将前端中后端 api 的路径改为了相对路径，而开发模式下前端与后端的端口不一样，导致应用无法在开发模式下正常运行。

所幸，我们可以通过 Vite 设置代理。在 `vite.config.js` 中：
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],

  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:3001',
        changeOrigin: true,
      },
    }
  },
})
```

此时，所有发向 `localhost:5173/api` 的请求都将自动转接到 `localhost:3001`，方便前后端完成通信。

此时，在前端的视角来看，所有的请求都是发向 `localhost:5173` 的，因此不会被同源策略限制，后端的 `cors` 中间件也就不需要了。