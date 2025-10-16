#OdinProject 
在 Ruby 中，假值只有 false 和 nil。

```ruby
if statement_to_be_evaluated == true
  # do something awesome...
end

if 1 < 2
  puts "Hot diggity, 1 is less than 2!"
end
#=> Hot diggity, 1 is less than 2!
```

```ruby 
puts "Hot diggity damn, 1 is less than 2" if 1 < 2
```

```ruby 
if attack_by_land == true
  puts "release the goat"
else
  puts "release the shark"
end
```

```ruby 
if attack_by_land == true
  puts "release the goat"
elsif attack_by_sea == true
  puts "release the shark"
else
  puts "release Kevin the flying octopus "
end
```

```ruby
5.eql?(5.0) #=> false; although they are the same value, one is an integer and the other is a float
5.eql?(5)   #=> true 
```

`equal?` 类似于 Python 中的 is。
```ruby
a = 4
b = 4
a.equal?(b) #=> true 

a = "hello"
b = "hello"
a.equal?(b) #=> false
```

`<=>` (spaceship operator) ，会比较两个变量的大小，并根据比较结果返回-1 0 或 1。
```ruby 
5 <=> 10    #=> -1
10 <=> 10   #=> 0
10 <=> 5    #=> 1
```