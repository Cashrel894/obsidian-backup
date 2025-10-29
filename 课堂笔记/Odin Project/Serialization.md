#OdinProject 
## YAML
一种具有高度可读性的序列化语言，适合用于作为配置文件使用。
```yaml
name: "David"
height: 124
age: 28
children:
  "John":
    age: 1
    height: 10
  "Adam":
    age: 2
    height: 20
  "Robert":
    age: 3
    height: 30
traits:
  - smart
  - nice
  - caring
```

在 Ruby 中使用 YAML：
```ruby 
require 'yaml'
YAML.load File.read('test.yaml')
YAML.dump {name => "David", height: 124, age: 28}
```

## JSON
与 YAML 相似，同样是一种高可读性的序列化语言。只不过，由于 JSON 发源于 JavaScript，它的写法更近似于 JS 中的对象表示。
```ruby 
require 'json'
JSON.load File.read('test.yaml')
JSON.dump {name => "David", height: 124, age: 28}
```
可以看出，在 Ruby 中 JSON 和 YAML 的使用方式可以说完全一致。

