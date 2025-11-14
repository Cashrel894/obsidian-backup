#OdinProject 
```ruby 
grade = 'C'

case grade
in 'A' then puts 'Amazing effort'
in 'B' then puts 'Good work'
in 'C' then puts 'Well done'
in 'D' then puts 'Room for improvement'
else puts 'See me'
end

# => Well done
```

```ruby
grade = 'C'

result = case grade
  in 'A' then 1
  in 'B' then 2
  in 'C' then 3
  else 0
end

puts result
#=> 3
```

## Variable Pattern
```ruby 
input = 3

case input
in String then puts 'input was of type String'
in Integer then puts 'input was of type Integer'
end

#=> input was of type Integer
```

> [!note] 
> 上述代码原理：分别计算 String == input 和 Integer == input。
> 这是因为 Ruby 重载了 String 和 Integer 的 `==` 方法，当参数为对应类的实例时返回 true。注意等号两侧不可颠倒。

```ruby
age = 15

case age
in a
  puts a
end

# => 15
```

```ruby
case 3
in 3 => a
  puts a
end

# => 3
```

```ruby
a = 5

case 1
in ^a # 使用^可以仅匹配变量a的值，而不是将提取的模式存入a
  :no_match
end

#=> NoMatchingPatternError
```

## Alternative Pattern
```ruby
case 0
in 0 | 1 | 2
  puts :match
end

# => match
```

## Guard conditions
```ruby
some_other_value = true

case 0
in 0 if some_other_value
  puts :match
end

# => match
```

## Array Pattern
```ruby
arr = [1, 2]

case arr
in [Integer, Integer]
  puts :match
in [String, String]
  puts :no_match
end

# => match
```

```ruby
arr = [1, 2, 3]

case arr
in [Integer, Integer]
  puts :no_match
end

# => [1, 2, 3] (NoMatchingPatternError)
```

使用 `*` 匹配任意数量的元素：
```ruby
arr = [1, 2, 3]

case arr
in [Integer, Integer, *]
  puts :match
end

# => match
```

```ruby
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

case arr
in [Integer, Integer, *, Integer, Integer]
  puts :match
end

# => match
```

将剩余元素存入变量：
```ruby
arr = [1, 2, 3, 4]

case arr
in [1, 2, *tail]
  p tail
end

# => [3, 4]
```

用 `_` 作为占位符，匹配确切数量的任意元素：
```ruby
arr = [1, 2, 3, 4]

case arr
in [_, _, 3, 4]
  puts :match
end

# => match
```

```ruby
case [1, 2, 3, [4, 5]]
in [1, 2, 3, [4, a] => arr]
  puts a
  p arr
end

# => 5
# => [4, 5]
```

在模式匹配中，最外层列表的方括号可以省略：
```ruby
arr = [1, 2, 3, 4]

case arr
in 1, 2, 3, 4
  puts :match
end

# => match
```

## Hash Pattern 
```ruby
case { a: 'apple', b: 'banana' }
in { a: 'aardvark', b: 'bat' }
  puts :no_match
in { a: 'apple', b: 'banana' }
  puts :match
end

# => match
```

```ruby
case { a: 'apple', b: 'banana' }
in { a: a, b: b }
  puts a
  puts b
end

# => apple
# => banana
```

当键名和要存入的变量名相同时，可以省略掉变量名：
```ruby
case { a: 'apple', b: 'banana' }
in { a:, b: }
  puts a
  puts b
end

# => apple
# => banana
```

同样，最外层的大括号也可以省略：
```ruby
case { a: 'apple', b: 'banana' }
in a:, b:
  puts a
  puts b
end

# => apple
# => banana
```

用 `**` 来捕获剩余的键值对组成的 Hash：
```ruby
case { a: 'ant', b: 'ball', c: 'cat' }
in { a: 'ant', **rest }
  p rest
end

# => { :b => "ball", :c => "cat" }
```

注意：Hash 只会检查与模式本身提供的键值对相关的部分：
```ruby
case { a: 'ant', b: 'ball' }
in { a: 'ant' }
  'this will match'
in { a: 'ant', b: 'ball' }
  'this will never be reached'
end
```

如果不想匹配含有多余键值对的 Hash，可以使用 `**nil`：
```ruby
case { a: 'ant', b: 'ball' }
in { a: 'ant', **nil }
  puts :no_match
in { a: 'ant', b: 'ball' }
  puts :match
end

# => match
```

