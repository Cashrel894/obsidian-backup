#OdinProject 
## yield
```ruby
def logger
  yield
end

logger { puts 'hello from the block' }
#=> hello from the block

logger do
  p [1,2,3]
end
#=> [1,2,3]
```

```ruby
def double_vision
  yield
  yield
end

double_vision { puts "How many fingers am I holding up?" }
#=> How many fingers am I holding up?
#=> How many fingers am I holding up?
```

```ruby
def love_language
  yield('Ruby')
  yield('Rails')
end

love_language { |lang| puts "I love #{lang}" }
#=> I love Ruby.
#=> I love Rails.
```

block 中参数的缺省值为 nil：
```ruby
def say_something
  yield # No arguments are passed to yield
end

say_something do |word| # But the block expects one argument to be passed in
  puts "And then I said: #{word}"
end
#=> And then I said:
```

### block_given?
```ruby
def maybe_block
  if block_given?
    puts "block party"
  end
  puts "executed regardless"
end

maybe_block
# => executed regardless

maybe_block {} # {} is just an empty block
# => block party
# => executed regardless
```

## lambdas
```ruby
my_lambda = lambda { puts "my lambda" }

my_other_lambda = -> { puts "hello from the other side" }


my_lambda = -> { puts "high five" }
my_lambda.call
# => high five
```

```ruby
my_name = ->(name) { puts "hello #{name}" }

my_age = lambda { |age| puts "I am #{age} years old" }


my_name.call("tim")
#=> hello tim
my_age.call(78)
#=> I am 78 years old
```

```ruby
my_lambda = ->(name="r2d2") { puts name }

my_lambda.call
# => r2d2
```

## Procs 
```ruby
a_proc = Proc.new { puts "this is a proc" }

a_proc.call
#=> this is a proc
```

```ruby
a_proc = proc { puts "this is a proc" }

a_proc.call
#=> this is a proc
```

```ruby
a_proc = Proc.new { |name, age| puts "name: #{name} --- age: #{age}" }

a_proc.call("tim", 80)
#=> name: tim --- age: 80
```

```ruby
my_proc = Proc.new { |name="bob"| puts name }

my_proc.call
# => bob
```

## Lambdas vs Procs 
- **参数**：proc 不关心传入参数的数量，少了就用 nil 作为缺省值，多了就直接忽略；而 lambda 在参数数量不匹配时会报错。
- **返回**：lambda 中使用 return 会跳出 lambda 语句块本身的执行，而 proc 中则会直接跳出*定义该 proc 的语句块*的执行。
```ruby
def outer_method
  a_proc = Proc.new { return }
  puts "this line will be printed"

  inner_method(a_proc)
  puts "this line is never reached"
end

def inner_method(proc)
  proc.call
  puts "this line is also never reached"
end

outer_method
#=> this line will be printed
```


## Capturing Blocks
```ruby
def cool_method(&my_block)
  my_block.call
end

cool_method { puts "cool" }
```

`&` 的作用是，显式地接受一个 block，或调用参数的 `to_proc` 方法，将其自动转换为一个 proc。
```ruby
arr = ["1", "2", "3"]

arr.map(&:to_i)
# => [1, 2, 3]
```

```ruby
def cool_method
  yield
end

my_proc = Proc.new { puts "proc party" }

cool_method(&my_proc)
# => proc party
```

## Binding
Binding 类一般用不上，但可以用来窥见 proc 和 lambda 实现闭包的方式：
```ruby
def return_binding
	foo = 100
	binding
end

# Foo is available thanks to the binding,
# even though we are outside of the method
# where it was defined.

puts return_binding.class
puts return_binding.eval('foo')

# If you try to print foo directly you will get an error.
# The reason is that foo was never defined outside of the method.

puts foo
```
