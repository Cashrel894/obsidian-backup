#OdinProject 
[[Semantic Versioning]]

按照惯例，一个 Ruby 项目应当将所有的 class 独立为一个文件，并将所有 Ruby 文件（除了 main. rb）放入 lib 文件夹中：
```
project_name
├── lib
│   └── lovely_file_of_yours.rb
└── main.rb
```

## require_relative
```ruby 
# You're in the root of the project, the directory that holds main.rb

# main.rb
require_relative 'lib/sort'

# sort.rb
require_relative 'sort/bubble_sort'
require_relative 'sort/bogo_sort'
require_relative 'sort/merge_sort'
```
![[../../附件/Pasted image 20251022204818.png]]

## require 
![[../../附件/Pasted image 20251022204859.png]]
通常，如果需要导入自己的代码，用 `require_relative`。否则如果用外部的代码，用 `require`。

> [!note] 
> 由于导入的文件都放在同一命名空间下，很可能会引发命名冲突。因此，可以用 module 包裹可能被导入的文件，并将函数设置为类方法，来避免这一问题。
```ruby
# all files are in the same directory for simplicity's sake

# not_so_green.rb
module NotSoGreen
  def self.food_opinion(food)
    "#{food} is awesome!"
  end
end
# scheals.rb
module Scheals
  def self.food_opinion(food)
    "#{food} is awful!"
  end
end
# main.rb
require_relative 'not_so_green'
require_relative 'scheals'

puts NotSoGreen.food_opinion('Cereal')
#=> Cereal is awesome!
puts Scheals.food_opinion('Marmite')
#=> Marmite is awful!
puts food_opinion('Cereal')
#=> Errors out—there's no longer a free floating food_opinion method to use.

```

