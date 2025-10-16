#OdinProject 
```ruby
desired_location = "Barcelona"
johns_location = desired_location

desired_location  #=> "Barcelona"
johns_location    #=> "Barcelona"
```

```ruby
johns_location.upcase  #=> "BARCELONA"

desired_location        #=> "Barcelona"
johns_location          #=> "BARCELONA"
```

```ruby
johns_location.upcase!  #=> "BARCELONA"

desired_location        #=> "BARCELONA"
johns_location          #=> "BARCELONA"
```

```ruby
name = gets # get string (like input() in Python)
name = gets.chomp # remove blank chars on both sides, like '\n'
```

```ruby
name = 'Somebody Else'

def print_full_name(first_name, last_name)
  name = first_name + ' ' + last_name
  puts name
end

print_full_name 'Peter', 'Henry'   # prints Peter Henry
print_full_name 'Lynn', 'Blake'    # prints Lynn Blake
print_full_name 'Kim', 'Johansson' # prints Kim Johansson
puts name                          # prints Somebody Else
```

## Variable Scope
```ruby
total = 0
[1, 2, 3].each { |number| total += number }
puts total # 6
```

```ruby
total = 0
[1, 2, 3].each do |number|
  total += number
end
puts total # 6
```

## Types of Variables
```ruby
MOD_CONSTANT = 1000000007 # Constant Variable
$var = "Hello!"  # Global Variable. Available everywhere. Don't use!
@@instances = 0 # Class Variable. Available to its instances and the class itself
@name = "John" # Instance Variable. Available to the instance itself
```