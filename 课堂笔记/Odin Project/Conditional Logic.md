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
puts "x is NOT 3" unless x == 3
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

```ruby 
grade = 'F'

did_i_pass = case grade #=> create a variable `did_i_pass` and assign the result of a call to case with the variable grade passed in
  when 'A' then "Hell yeah!"
  when 'D' then "Don't tell your mother."
  else "'YOU SHALL NOT PASS!' -Gandalf"
end
```

```ruby 
grade = 'F'

case grade
when 'A'
  puts "You're a genius"
  future_bank_account_balance = 5_000_000
when 'D'
  puts "Better luck next time"
  can_i_retire_soon = false
else
  puts "'YOU SHALL NOT PASS!' -Gandalf"
  fml = true
end
```

```ruby 
age = 19
unless age < 18
  puts "Get a job."
end
```

```ruby 
age = 19
response = age < 18 ? "You still have your entire life ahead of you." : "You're all grown up."
puts response #=> "You're all grown up."

```