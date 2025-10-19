#OdinProject 
```ruby 
i = 0
loop do
  puts "i is #{i}"
  i += 1
  break if i == 10
end
```

```ruby 
i = 0
loop do
  i = i + 2
  if i == 4
    next        # skip rest of the code in this iteration
  end
  puts i
  if i == 10
    break
  end
end
```

```ruby 
i = 0
while i < 10 do
 puts "i is #{i}"
 i += 1
end

while gets.chomp != "yes" do
  puts "Are we there yet?"
end
```

```ruby 
begin
  puts "Do you want to do that again?"
  answer = gets.chomp
end while answer == 'Y'
```

```ruby 
i = 0
until i >= 10 do
 puts "i is #{i}"
 i += 1
end

until gets.chomp == "yes" do
  puts "Do you like Pizza?"
end
```

```ruby 
(1..5)      # inclusive range: 1, 2, 3, 4, 5
(1...5)     # exclusive range: 1, 2, 3, 4

# We can make ranges of letters, too!
('a'..'d')  # a, b, c, d

for i in 0..5
  puts "#{i} zombies incoming!"
end
```

```ruby 
x = [1, 2, 3, 4, 5]

for i in x.reverse do
  puts i
end

puts "Done!"
```

```ruby 
5.times do
  puts "Hello, world!"
end
```

```ruby 
5.times do |number|
  puts "Alternative fact number #{number}"
end
```

```ruby 
5.upto(10) { |num| print "#{num} " }     #=> 5 6 7 8 9 10

10.downto(5) { |num| print "#{num} " }   #=> 10 9 8 7 6 5
```

```ruby 
names = ['Bob', 'Joe', 'Steve', 'Janice', 'Susan', 'Helen']

names.each { |name| puts name }
```

```ruby 
names = ['Bob', 'Joe', 'Steve', 'Janice', 'Susan', 'Helen']
x = 1

names.each do |name|
  puts "#{x}. #{name}"
  x += 1
end
```