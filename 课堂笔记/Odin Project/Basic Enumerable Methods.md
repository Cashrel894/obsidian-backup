#OdinProject 
```ruby 
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.select { |friend| friend != 'Brian' }
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

```ruby 
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.reject { |friend| friend == 'Brian' }
 #=> ["Sharon", "Leo", "Leila", "Arun"]
```

```ruby 
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.each { |friend| puts "Hello, " + friend }

#=> Hello, Sharon
#=> Hello, Leo
#=> Hello, Leila
#=> Hello, Brian
#=> Hello, Arun

#=> ["Sharon", "Leo", "Leila", "Brian" "Arun"]
```

```ruby 
my_array = [1, 2]

my_array.each do |num|
  num *= 2
  puts "The new number is #{num}."
end

#=> The new number is 2.
#=> The new number is 4.

#=> [1, 2]
```

```ruby 
my_hash = { "one" => 1, "two" => 2 }

my_hash.each { |key, value| puts "#{key} is #{value}" }

#=> one is 1
#=> two is 2
#=> { "one" => 1, "two" => 2}
```

```ruby 
fruits = ["apple", "banana", "strawberry", "pineapple"]

fruits.each_with_index { |fruit, index| puts fruit if index.even? }

#=> apple
#=> strawberry
#=> ["apple", "banana", "strawberry", "pineapple"]
```

```ruby 
friends = ['Sharon', 'Leo', 'Leila', 'Brian', 'Arun']

friends.map { |friend| friend.upcase }
#=> `['SHARON', 'LEO', 'LEILA', 'BRIAN', 'ARUN']`
```

```ruby 
array = %w(a b c)

array.map.with_index { |ch, idx| [ch, idx] }

# [["a", 0], ["b", 1], ["c", 2]]
```

```ruby 
["11", "21", "5"].map(&:to_i)
["orange", "apple", "banana"].map(&:class)
```

```ruby 
my_numbers = [5, 6, 7, 8]

my_numbers.reduce { |sum, number| sum + number }
#=> 26
```

```ruby 
my_numbers = [5, 6, 7, 8]

my_numbers.reduce(1000) { |sum, number| sum + number }
#=> 1026
```

```ruby 
.any?
.all?
.none?
.include?
.one?
```

```ruby 
> names = ["James", "Bob", "Joe", "Mark", "Jim"]
> names.group_by{|name| name.length}
=> {5=>["James"], 3=>["Bob", "Joe", "Jim"], 4=>["Mark"]}
```

```ruby 
> names.grep(/J/)
=> ["James", "Joe", "Jim"]
```