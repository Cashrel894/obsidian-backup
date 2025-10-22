#OdinProject 
```ruby 
irb :001 > "hello".class
=> String
irb :002 > "world".class
=> String
```

```ruby 
class GoodDog
end

sparky = GoodDog.new
```

```ruby 
module Speak
  def speak(sound)
    puts sound
  end
end

class GoodDog
  include Speak
end

class HumanBeing
  include Speak
end

sparky = GoodDog.new
sparky.speak("Arf!")        # => Arf!
bob = HumanBeing.new
bob.speak("Hello!")         # => Hello!
```

```ruby 
module Speak
  def speak(sound)
    puts "#{sound}"
  end
end

class GoodDog
  include Speak
end

class HumanBeing
  include Speak
end

puts "---GoodDog ancestors---"
puts GoodDog.ancestors
puts ''
puts "---HumanBeing ancestors---"
puts HumanBeing.ancestors
```

```ruby 
---GoodDog ancestors---
GoodDog
Speak
Object
Kernel
BasicObject

---HumanBeing ancestors---
HumanBeing
Speak
Object
Kernel
BasicObject
```

## Initialize
```ruby 
class GoodDog
  def initialize
    puts "This object was initialized!"
  end
end

sparky = GoodDog.new        # => "This object was initialized!"
```

```ruby 
class GoodDog
  def initialize(name)
    @name = name
  end
end

sparky = GoodDog.new("Sparky")
```

## Composition
```ruby 
class Engine
  def start
    puts "Engine starting..."
  end
end

class Car
  def initialize
    @engine = Engine.new  # Engine instance is created when Car is created
  end

  def start
    @engine.start
  end
end

my_car = Car.new
my_car.start  # Engine is an integral part of Car
```
## Aggregation 
```ruby 
class Passenger
end

class Car
  def initialize(passengers)
    @passengers = passengers  # Passengers are given to the Car at creation
  end
end

# Passengers can exist without Car
passengers = [Passenger.new, Passenger.new]
my_car = Car.new(passengers)
```

>  **Composition:** The container owns the contained objects, and their lifecycles are tightly linked.
>  **Aggregation:** The container does not own the contained objects; they can exist independently.

## Instance Methods 
```ruby 
class GoodDog
  def initialize(name)
    @name = name
  end

  def speak
    "Arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
```

## Accessor Methods 
在 Ruby 中，不能直接获取实例变量，而是需要通过调用实例方法间接获取：
```ruby 
puts sparky.name
```

```ruby 
NoMethodError: undefined method `name' for #<GoodDog:0x007f91821239d0 @name="Sparky">
```

```ruby 
class GoodDog
  def initialize(name)
    @name = name
  end

  def get_name
    @name
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.get_name
```

要修改实例变量也是同理：
```ruby 
class GoodDog
  def initialize(name)
    @name = name
  end

  def get_name
    @name
  end

  def set_name=(name)
    @name = name
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.get_name
sparky.set_name = "Spartacus"
puts sparky.get_name
```
方法名最后的 `=` 使 ruby 将该方法视作 setter 方法。

通常，getter 和 setter 方法的名字和要访问的属性名一致：
```ruby 
class GoodDog
  def initialize(name)
    @name = name
  end

  def name                  # This was renamed from "get_name"
    @name
  end

  def name=(n)              # This was renamed from "set_name="
    @name = n
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.name            # => "Sparky"
sparky.name = "Spartacus"
puts sparky.name            # => "Spartacus"
```

> [!note] 
> setter 方法总是返回它的参数，其他返回值都会被忽略。

这样访问实例变量也太麻烦了，所以 ruby 又提供了一种替代方式：
```ruby 
class GoodDog
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} says arf!"
  end
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.name            # => "Sparky"
sparky.name = "Spartacus"
puts sparky.name            # => "Spartacus"
```
单独设置 `attr_getter` 和 `attr_setter` 也是可以的。

```ruby 
attr_accessor :name, :height, :weight
```

**注意**：以下写法不会调用 setter，而是会被视作初始化局部变量，因此不会改变属性的值。
```ruby 
def change_info(n, h, w)
  name = n
  height = h
  weight = w
end
```
因此需要使用 self 调用 setter
```ruby 
def change_info(n, h, w)
  self.name = n
  self.height = h
  self.weight = w
end
```

## Class Methods 
```ruby 
# ... rest of code ommitted for brevity

def self.what_am_i         # Class method definition
  "I'm a GoodDog class!"
end

GoodDog.what_am_i          # => I'm a GoodDog class!
```

## Class Variables 
```ruby 
class GoodDog
  @@number_of_dogs = 0

  def initialize
    @@number_of_dogs += 1
  end

  def self.total_number_of_dogs
    @@number_of_dogs
  end
end

puts GoodDog.total_number_of_dogs   # => 0

dog1 = GoodDog.new
dog2 = GoodDog.new

puts GoodDog.total_number_of_dogs   # => 2
```

## Constants 
```ruby 
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age  = a * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
puts sparky.age             # => 28
```

## Inheritance 
```ruby 
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
end

class Cat < Animal
end

sparky = GoodDog.new
paws = Cat.new
puts sparky.speak           # => Hello!
puts paws.speak             # => Hello!
```

```ruby 
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
  def speak
    super + " from GoodDog class"
  end
end

sparky = GoodDog.new
sparky.speak        # => "Hello! from GoodDog class"
```

注意：在 `initialize` 中，如果在不使用括号的情况下调用 `super` ，会自动将子类的全部参数上传。这就意味着可能会导致一些 bug。
```ruby 
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

class GoodDog < Animal
  def initialize(color)
    super
    @color = color
  end
end

bruno = GoodDog.new("brown")        # => #<GoodDog:0x007fb40b1e6718 @color="brown", @name="brown">
```

```ruby 
class Animal 
  attr_accessor :name 
  def initialize(name) 
    @name = name 
  end 
end

class BadDog < Animal
  def initialize(age, name)
    super(name)
    @age = age
  end
end

BadDog.new(2, "bear")        # => #<BadDog:0x007fb40b2beb68 @age=2, @name="bear">
```

## Mixing in Modules 
```ruby 
module Swimmable
  def swim
    "I'm swimming!"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable         # mixing in Swimmable module
end

class Mammal < Animal
end

class Cat < Mammal
end

class Dog < Mammal
  include Swimmable         # mixing in Swimmable module
end
```

ruby 查找方法时，总是先自身的方法开始，接着从 include 的最后一个 module 查到第一个，最后才查继承的 superclass。
```ruby 
class GoodDog < Animal
  include Swimmable
  include Climbable
end

puts "---GoodDog method lookup---"
puts GoodDog.ancestors
```

```ruby 
---GoodDog method lookup---
GoodDog
Climbable
Swimmable
Animal
Walkable
Object
Kernel
BasicObject
```

## More Modules
Module 还有其他的作用：

比如，可以作为 namespace 使用：
```ruby 
module Mammal
  class Dog
    def speak(sound)
      p "#{sound}"
    end
  end

  class Cat
    def say_name(name)
      p "#{name}"
    end
  end
end
```

```ruby 
buddy = Mammal::Dog.new
kitty = Mammal::Cat.new
buddy.speak('Arf!')           # => "Arf!"
kitty.say_name('kitty')       # => "kitty"
```

另一个是作为方法的容器：
```ruby 
module Conversions
  def self.farenheit_to_celsius(num)
    (num - 32) * 5 / 9
  end
end
```

```ruby 
value = Conversions.farenheit_to_celsius(32) # better

value = Conversions::farenheit_to_celsius(32)
```

## Private, Protected, and Public (Method Access Control)
```ruby 
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age = a
  end

  private

  def human_years
    age * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
sparky.human_years
```

```ruby 
NoMethodError: private method `human_years' called for
  #<GoodDog:0x007f8f431441f8 @name="Sparky", @age=4>
```

```ruby 
class Person
  def initialize(age)
    @age = age
  end

  def older?(other_person)
    age > other_person.age
  end

  protected

  attr_reader :age
end

malory = Person.new(64)
sterling = Person.new(42)

malory.older?(sterling)  # => true
sterling.older?(malory)  # => false

malory.age
  # => NoMethodError: protected method `age' called for #<Person: @age=64>
```

## Some Object Methods 
```ruby 
.class 
.superclass 
.ancestors 
.instance_of?
```
编程时需要注意不要覆盖掉 Object 内置方法。