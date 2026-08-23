#ctf 

## Secure Chat 1
通读 `chat-server` 和 `run` 后，可以注意到：
- `chat-server` 存在多处 SQL 注入可能，除只能从本地登录的管理员账号 `alice` 外都可以免密登录（只要知道用户名）。
- Sharon 的用户名后缀由一串未知的数字组成。
- Flag 最初由 Sharon 明文发送到 Bob，想到可以以 Bob 的身份查看与 Sharon 的聊天，但是需要获取 Sharon 的真实用户名。
- Alice 在接收到 Mallory 的提醒后才会与 Bob 进行加密通信，同时 Bob 也会向 Alice 发送 Flag。

综合以上信息，不难想到一个相对简单的解法：
1. 获取 Sharon 用户名：在 `/login` 路径向 `username` 注入 `1' OR SUBSTR(username, 1, 6)='sharon' --`，以 Sharon 的身份登录，根路径会显示 Sharon 的完整用户名。
2. 以 Bob 身份查看明文 Flag：依旧 `/login` 向 `username` 注入 `bob' --`，接着 `/chat/<Sharon完整用户名>` 查看 Flag。

## Secure Chat 2
这一次 Sharon 会在向 Bob 发送 Flag 后删号跑路，前一次的解法就行不通了。于是可以想到利用 Alice 与 Bob 的通信，伪装成 Alice 获取 Bob 发送的 Flag。

但是这里面临一个问题：Alice 的账号 `alice` 为管理员，只能从服务器本地地址（即 Alice 的 host ）登录。

此时注意到 `chat_server` 提供了修改用户 `username`、`password`、`admin` 的功能，那么只需要让管理员 Alice 主动去除自身的 `admin` 权限，就可以登录 `alice` 与 Bob 加密通信了。

而 Alice 初始会不断检查与 Mallory 的通信，直到收到 `I heard someone shared the flag with Bob!` 为止。于是可以想到先以 Mallory 的名义对 Alice 发动 XXS 攻击，使得 Alice 修改自身的 `admin` 权限（以及 `password`，方便后续登录 `alice`，同时阻止 Alice 本人登录，污染与 Bob 的通信）。

于是具体攻击流程如下：
- 发动 XXS 攻击：SQL 注入登录 `mallory`，向 `alice` 发送 XXS 攻击脚本，再发送`I heard someone shared the flag with Bob!`启动Alice与Bob的加密通信。
```html
<script>
    fetch("/user/alice/modify", {
        method: "POST",
        headers: {
            "Content-Type": "application/x-www-form-urlencoded",
        },
        body: new URLSearchParams({
            username: "alice",
            password: "123456",
            admin: "false",
        })
    });
</script>
```
- 通信身份伪装：用修改的密码登录 `alice`，根据 `run` 的通信流程与 `bob` 建立 DH-AES 加密信道，获取 Bob 发送的 Flag。