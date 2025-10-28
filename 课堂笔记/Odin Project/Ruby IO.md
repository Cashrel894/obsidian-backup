#CS61C 
## Unix I/O
![[../../附件/Pasted image 20251028205619.png]]

Unix 系统主要提供三种 I/O 流：
- stdin (`/dev/fd/0`)
- stdout (`/dev/fd/1`)
- stderr (`/dev/fd/2`)
其中，fd 指的是 `file descriptor`。

默认情况下，stdin 总是键盘输入，而 stdout 和 stderr 则输出到终端上。

## Ruby IO Class 
Ruby 的 IO 类提供了三个与 IO 相关的常量：`STDIN`、`STDOUT`、`STDERR`，它们都是 `IO` 类的实例，包含了上述的标准流。

默认情况下，全局变量 `$stdin` `$stdout` `$stderr` 分别指向了这三个常量。

我们之前一直使用的 `puts` 和 gets 其实分别是 `$stdout.puts` 和 `$stdin.gets` 的简写。

我们也可以创建自己的 IO 实例：
```ruby 
> io = IO.new(1) # stdout
=> #<IO:fd 1>
> io.puts 'hello world'
hello world
```

```ruby 
> fd = IO.sysopen('dev/null', 'w+')
=> 8
> dev_null = IO.new(fd)
=> # <IO:fd 8>
> dev_null.puts 'hello'
=> nil
> dev_null.gets 
=> nil
> dev_null.close 
=> nil 
```

