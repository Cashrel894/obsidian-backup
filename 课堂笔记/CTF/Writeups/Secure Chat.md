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
1. 发动 XSS 攻击：SQL 注入登录 `mallory`，向 `alice` 发送 XXS 攻击脚本，再发送 `I heard someone shared the flag with Bob!` 启动Alice与Bob的加密通信。
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
2. 通信身份伪装：用修改的密码登录 `alice`，根据 `run` 的通信流程与 `bob` 建立 DH-AES 加密信道，获取 Bob 发送的 Flag。

## Secure Chat 3
这一次 Bob 不会再向 Alice 发送 flag 了，必须另寻他计。

观察代码可以发现，唯一能提取 flag 的地方只剩下 Bob 与 Sharon 的通信，而虽然 Sharon 已经删号，只要 Bob 没有删号，数据库仍然会保留她与 Bob 的通信内容。

而通过对 `/login` 进行 SQL 注入，可以泄露 `encrypted_chats` 的任意数据，例如这样注入：
```sql
1' UNION SELECT encrypted_contents, '123456', FALSE FROM encrypted_chats WHERE encrypted_username_1 IS NULL OR encrypted_username_2 IS NULL --
```
就可以获得含有 flag 的那条通信内容。

问题在于，这条消息受到 AES-ECB 加密保护，密钥来自服务端的 secret_key，常规手段无法获取，如何解决？

借由 ECB 的特点，不难想到可以发动前缀填充攻击，而 `/user/<username>/modify` 为我们提供了攻击的锚点。修改用户名时，会一并修改目标用户参与的所有加密消息中的前缀 `<username>: `；而删除用户时则不会对原消息加以改动。

但 Sharon 已经销号，怎么让 `modify` 定位到那条消息呢？可以先把 Bob 的用户名改为 Sharon 的原用户名，这样服务端就会误以为是原 `bob` 发送了那条前缀为 `<Sharon原用户名>: ` 的消息，此时再修改 Bob 的用户名为想要的前缀即可。

这里又引发一个问题：只有管理员才有权限修改用户名，我们必须借由对管理员 Alice 的 XSS 攻击才能做到，而若使用 2 中的方法，Alice 在获取 Sharon 完整用户名前需要剥除自己的管理员权限，这样就没办法改用户名了。

于是为了在保留 `alice` 权限的前提下获取 Sharon 用户名，就必须修改收发方式：不是以 `alice` 的身份登录，而是通过 XSS 发送，同时用 `bob` 的账号接收。为防止 Alice 本人上号污染通信，在开始前需要 XSS 修改 `alice` 的密码。

PS：这里试错了很久才想到这样做，此前尝试过建一个假号、用 XSS 把用户名改成 `alice`，但若不修改密码会有污染问题，修改密码则会导致假 `alice` 获得管理员权限/真 `alice` 失去管理员权限，尝试很久未能解决，所以应该不是好方法。

于是我们现在获得了所有不同长度 padding 的加密消息，接下来考虑如何得到任意 `block` 的加密。其实可以沿用之前获得加密消息的思路，以明文创建账号、向自己发送消息、SQL 注入获取收发用户相同通信的加密用户名 1、删除账号（此时那条消息也会从数据库中抹除，不用担心与后续的加密竞争）：
```sql
1' UNION SELECT encrypted_username_1, '123456', FALSE FROM encrypted_chats WHERE encrypted_username_1=encrypted_username_2 --
```

综上，我们就具备了 CPA 攻击的全部条件，执行解密即可。