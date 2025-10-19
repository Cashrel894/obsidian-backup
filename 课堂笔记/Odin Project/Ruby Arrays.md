#OdinProject 
```ruby 
Array.new               #=> []
Array.new(3)            #=> [nil, nil, nil]
Array.new(3, 7)         #=> [7, 7, 7]
Array.new(3, true)      #=> [true, true, true]
```

```ruby 
str_array = ["This", "is", "a", "small", "array"]

str_array[0]            #=> "This"
str_array[1]            #=> "is"
str_array[4]            #=> "array"
str_array[-1]           #=> "array"
str_array[-2]           #=> "small"
```

```ruby 
str_array = ["This", "is", "a", "small", "array"]

str_array.first         #=> "This"
str_array.first(2)      #=> ["This", "is"]
str_array.last(2)       #=> ["small", "array"]
```

```ruby 
num_array = [1, 2]

num_array.push(3, 4)      #=> [1, 2, 3, 4]
num_array << 5            #=> [1, 2, 3, 4, 5]
num_array.pop             #=> 5
num_array                 #=> [1, 2, 3, 4]
```

```ruby 
num_array = [2, 3, 4]

num_array.unshift(1)      #=> [1, 2, 3, 4]
num_array.shift           #=> 1
num_array                 #=> [2, 3, 4]
```

```ruby 
num_array = [1, 2, 3, 4, 5, 6]

num_array.pop(3)          #=> [4, 5, 6]
num_array.shift(2)        #=> [1, 2]
num_array                 #=> [3]
```

```ruby 
a = [1, 2, 3]
b = [3, 4, 5]

a + b         #=> [1, 2, 3, 3, 4, 5]
a.concat(b)   #=> [1, 2, 3, 3, 4, 5]
```

```ruby 
[1, 1, 1, 2, 2, 3, 4] - [1, 4]  #=> [2, 2, 3]
```

```ruby 
num_array.methods       #=> A very long list of methods

[].empty?               #=> true
[[]].empty?             #=> false
[1, 2].empty?           #=> false

[1, 2, 3].length        #=> 3

[1, 2, 3].reverse       #=> [3, 2, 1]

[1, 2, 3].include?(3)   #=> true
[1, 2, 3].include?("3") #=> false

[1, 2, 3].join          #=> "123"
[1, 2, 3].join("-")     #=> "1-2-3"
```

```ruby 
irb :001 > a = [1, 2, 3, 4] 
=> [1, 2, 3, 4] 
irb :002 > a.map { |num| num**2 } 
=> [1, 4, 9, 16] 
irb :003 > a.collect { |num| num**2 } 
=> [1, 4, 9, 16] 
irb :004 > a 
=> [1, 2, 3, 4]
```

```ruby 
irb :005 > a.delete_at(1) 
=> 2 
irb :006 > a 
=> [1, 3, 4]
```

```ruby 
irb :007 > my_pets = ["cat", "dog", "bird", "cat", "snake"] 
=> ["cat", "dog", "bird", "cat", "snake"] 
irb :008 > my_pets.delete("cat") 
=> "cat" 
irb :009 > my_pets 
=> ["dog", "bird", "snake"]
```

```ruby 
irb :010 > b = [1, 1, 2, 2, 3, 3, 4, 4] 
=> [1, 1, 2, 2, 3, 3, 4, 4] 
irb :011 > b.uniq => [1, 2, 3, 4] 
irb :012 > b => [1, 1, 2, 2, 3, 3, 4, 4]
```

```ruby 
irb :013 > b = [1, 1, 2, 2, 3, 3, 4, 4] 
=> [1, 1, 2, 2, 3, 3, 4, 4] 
irb :014 > b.uniq! 
=> [1, 2, 3, 4] 
irb :015 > b 
=> [1, 2, 3, 4]
```

```ruby 
irb :001 > numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
irb :002 > numbers.select { |number| number > 4 } 
=> [5, 6, 7, 8, 9, 10] 
irb :003 > numbers 
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

```ruby 
irb: 001 > a = [1, 2, [3, 4, 5], [6, 7]] 
=> [1, 2, [3, 4, 5], [6, 7]] 
irb: 002 > a.flatten 
=> [1, 2, 3, 4, 5, 6, 7]
```

```ruby 
irb: 001 > a = [1, 2, 3, 4, 5] 
=> [1, 2, 3, 4, 5] 
irb: 002 > a.each_index { |i| puts "This is index #{i}" } 
This is index 0 
This is index 1 
This is index 2 
This is index 3 
This is index 4 
=> [1, 2, 3, 4, 5]
```

```ruby 
irb: 001 > a = [1, 2, 3, 4, 5] 
=> [1, 2, 3, 4, 5] 
irb: 002 > a.each_with_index { |val, idx| puts "#{idx+1}. #{val}" } 
1. 1 
2. 2 
3. 3 
4. 4 
5. 5 => [1, 2, 3, 4, 5]
```

```ruby 
irb :001 > a = [5, 3, 8, 2, 4, 1] 
=> [5, 3, 8, 2, 4, 1] 
irb :002 > a.sort 
=> [1, 2, 3, 4, 5, 8]
```

```ruby 
irb :001 > [1, 2, 3].product([4, 5]) 
=> [[1, 4], [1, 5], [2, 4], [2, 5], [3, 4], [3, 5]]
```