#OdinProject
## Numbers
```ruby
# Addition
1 + 1   #=> 2

# Subtraction
2 - 1   #=> 1

# Multiplication
2 * 2   #=> 4

# Division
10 / 5  #=> 2

# Exponent
2 ** 2  #=> 4
3 ** 4  #=> 81

# Modulus (find the remainder of division)
8 % 2   #=> 0  (8 / 2 = 4; no remainder)
10 % 4  #=> 2  (10 / 4 = 2 with a remainder of 2)

17 / 5.0  #=> 3.4

# To convert an integer to a float:
13.to_f   #=> 13.0

# To convert a float to an integer:
13.0.to_i #=> 13
13.9.to_i #=> 13

6.even? #=> true
7.even? #=> false

6.odd? #=> false
7.odd? #=> true

```

```ruby
16.divmod(5) #=>[3, 1]
16.remainder(5) #=>1
```
![[../../附件/Pasted image 20251015093221.png]]

## Strings
https://docs.ruby-lang.org/en/3.4/String.html
```ruby
# With the plus operator:
"Welcome " + "to " + "Odin!"    #=> "Welcome to Odin!"

"1" + 1 # Error!

# With the shovel operator:
"Welcome " << "to " << "Odin!"  #=> "Welcome to Odin!"

# With the concat method:
"Welcome ".concat("to ").concat("Odin!")  #=> "Welcome to Odin!"


"hello"[0]      #=> "h"

"hello"[0..1]   #=> "he"

"hello"[0, 4]   #=> "hell"

"hello"[-1]     #=> "o"
# More on https://docs.ruby-lang.org/en/3.4/String.html#class-String-label-String+Slices


\\  #=> Need a backslash in your string?
\b  #=> Backspace
\r  #=> Carriage return, for those of you that love typewriters
\n  #=> Newline. You'll likely use this one the most.
\s  #=> Space
\t  #=> Tab
\"  #=> Double quotation mark
\'  #=> Single quotation mark

name = "Odin"
puts "Hello, #{name}" #=> "Hello, Odin"
puts 'Hello, #{name}' #=> "Hello, #{name}"

"hello".capitalize #=> "Hello"

"hello".include?("lo")  #=> true
"hello".include?("z")   #=> false

"hello".upcase  #=> "HELLO"

"Hello".downcase  #=> "hello"

"hello".empty?  #=> false
"".empty?       #=> true

"hello".length  #=> 5

"hello".reverse  #=> "olleh"

"hello world".split  #=> ["hello", "world"]
"hello".split("")    #=> ["h", "e", "l", "l", "o"]

" hello, world   ".strip  #=> "hello, world"

"he77o".sub("7", "l")           #=> "hel7o"

"he77o".gsub("7", "l")          #=> "hello"

"hello".insert(-1, " dude")     #=> "hello dude"

"hello world".delete("l")       #=> "heo word"

"!".prepend("hello, ", "world") #=> "hello, world!"

5.to_s        #=> "5"

nil.to_s      #=> ""

:symbol.to_s  #=> "symbol"

"1".to_i #=> 1
"4".to_f #=> 4.0
"4 hi there".to_i #=> 4
"hi there".to_i #=> 0
"hi there again".to_f #=> 0.0
```

## Symbols
```ruby
:my_symbol

"string" == "string"  #=> true
"string".object_id == "string".object_id  #=> false
:symbol.object_id == :symbol.object_id    #=> true
```

## Booleans
```ruby
true
false 
nil

.nil?

4 == 5

'4' == 4 #=>false. Comparing two types!
```