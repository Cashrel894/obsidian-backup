#Missing 
在 UNIX 系统中，通常使用一种叫作 `signal` 的通信机制来实现不同进程之间的交流协作。
## 杀死进程
在 shell 中运行一条指令，随后在运行结束之前可以使用 `Ctrl-C` 中止程序运行。

本质上，这是向进程发送了 `SIGINT`（SIGnal INTterrupt），从而导致进程中止。

也可以使用 `Ctrl-\` 向进程发送 `SIGQUIT`。

此外，亦可以用 `kill -TERM <PID>` 发送 `SIGTERM`。

以上三种 signal 作用相似，大部分情况下都可以中止进程的运行，但进程可能可以无视它们继续运行。

而 `SIGKILL` 信号无法被无视，总是会立即中止进程。然而，这可能会导致一些副作用（如将其子进程变成孤儿进程），因此不建议使用。

## 暂停进程 & 后台管理
使用 `Ctrl-Z` 可以暂停当前终端进程。本质上，这是发送了 `SIGTSTP` 信号。(SIGnal Terminal SToP)

接着，就可以使用 `fg` 或 `bg` 来决定应该将进程放在前台还是后台继续运行。另外在命令中加入 `&` 后缀即使该命令在后台运行。

此外，可以用 `jobs` 命令查看与当前会话相关的、未完成的工作。

当终端关闭时，会向终端的所有子进程发送 `SIGHUP`，从而中止程序。不过，可以用 `nohup` 命令运行程序，使它无视 `SIGHUP`。

```
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

$ bg %1
[1]  - 18653 continued  sleep 1000

$ jobs
[1]  - running    sleep 1000
[2]  + running    nohup sleep 2000

$ kill -STOP %1
[1]  + 18653 suspended (signal)  sleep 1000

$ jobs
[1]  + suspended (signal)  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ jobs
[2]  + running    nohup sleep 2000

$ kill -SIGHUP %2

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000

$ jobs
```