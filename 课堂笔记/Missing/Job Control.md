#Missing 
在 UNIX 系统中，通常使用一种叫作 `signal` 的通信机制来实现不同进程之间的交流协作。
## 杀死进程
在 shell 中运行一条指令，随后在运行结束之前可以使用 `Ctrl-C` 中止程序运行。

本质上，这是向进程发送了 `SIGINT`（SIGnal INTterrupt），从而导致进程中止。

也可以使用 `Ctrl-\` 向进程发送 `SIGQUIT`。

此外，亦可以用 `kill -TERM <PID>` 发送 `SIGTERM`。

以上三种 signal 作用相似，大部分情况下都可以中止进程的运行。

## 暂停进程 & 后台管理
使用 `Ctrl-Z` 可以暂停当前终端进程。本质上，这是发送了 `SIGTSTP` 信号。(SIGnal Terminal SToP)

接着，就可以使用 `fg` 或 `bg` 来决定应该将进程放在前台还是后台继续运行。

此外，

