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

我们可以对 IO 流进行一些操作：
```ruby
[1] pry(main)> IO.sysopen '/Users/joelquenneville/Desktop/lorem.txt'
=> 8
[2] pry(main)> lorem = IO.new(8)
=> #<IO:fd 8>
[3] pry(main)> lorem.gets
=> "Lorem ipsum\n"
[4] pry(main)> lorem.pos
=> 12
[5] pry(main)> lorem.gets
=> "dolor\n"
[6] pry(main)> lorem.gets
=> "sit amet...\n"
[7] pry(main)> lorem.pos
=> 30
[8] pry(main)> lorem.gets
=> nil
[9] pry(main)> lorem.eof?
=> true
[10] pry(main)> lorem.rewind
=> 0
[11] pry(main)> lorem.pos
=> 0
```

```ruby 
[1] pry(main)> fd = IO.sysopen '/Users/joelquenneville/Desktop/test.txt', 'w+'
=> 8
[2] pry(main)> io = IO.new(fd)
=> #<IO:fd 8>
[3] pry(main)> io.puts 'hello world'
=> nil
[4] pry(main)> io.puts 'goodbye world'
=> nil
[5] pry(main)> io.gets
=> nil
[6] pry(main)> io.eof?
=> true
[7] pry(main)> io.rewind
=> 0
[8] pry(main)> io.gets
=> "hello world\n"
[9] pry(main)> io.pos
=> 12
[10] pry(main)> io.puts "middle"
=> nil
[11] pry(main)> io.rewind
=> 0
[12] pry(main)> io.read
=> "hello world\nmiddle\n world\n"
```

## Subclasses of class IO
- `File`
```ruby
f = File.open('langs.txt', 'w')
f.puts "Ruby"
f.close
```

```ruby 
File.open('langs.txt', 'w') do |f|
	f.puts "Ruby"
	f.write "Java\n"
	f << "Python\n"
end
```

```ruby 
File.exist? 'tempfile'
File.new 'tempfile', 'w'
File.mtime 'tempfile'
f.size 
File.rename 'tempfile', 'tempfile2'
```

```ruby 
while line = f.gets do 
	puts line 
end

File.readlines(fname).each do |line|
	puts line 
end 
```
- `Socket` `TCPSocket` 等
- `StringIO` (实际上 `StringIO` 和 `Socket` 并不是 IO 的子类，但用法上极其相似)
```ruby 
[1] pry(main)> string_io = StringIO.new('hello world')
=> #<StringIO:0x007feacb0cd4e8>
[2] pry(main)> string_io.gets
=> "hello world"
[3] pry(main)> string_io.puts 'goodby world'
=> nil
[4] pry(main)> string_io.rewind
=> 0
[5] pry(main)> string_io.read
=> "hello worldgoodby world\n"
```
- `Tempfile`
- `Dir`
```ruby 
Dir.mkdir 'tmp'
Dir.exist? 'tmp'
Dir.pwd
Dir.chdir 'tmp'
Dir.rmdir 'tmp'
fls = Dir.entries '.'
fls.inspect
Dir.home
Dir.home 'root'
```

## Executing external programs
```ruby 
system 'ls'
`ls`
%x[ls]
```

```ruby 
f = open("!ls -l |head -3")
puts $?.success?
```

## Redirecting
```ruby 
$stdout = File.open 'output.log', 'a'
puts 'Java'
$stdout.close
$stdout = STDOUT 
```