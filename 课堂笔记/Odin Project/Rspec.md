#OdinProject 
## Basics
```ruby
describe String do
  context '...' do
    it '...' do
      expect('blue').to eq('blue')
    end
    
    ...
  end
  
  ...
end
```

## Comparators
```ruby
describe Number do
  it '...' do
    expect(3).to be >= 2
  end
  
  it '...' do
    expect(3).to be_positive.and be < 10
  end
end
```

## let
```ruby
describe String do
  let(:favorite_color) { String.new('blue') }
	
  context '...' do
    it '...' do
      expect(favorite_color).to eq('blue') # 到此处才调用let中定义的favorite_color
    end
  end
end
```
同时，let 可以在任意块中定义，并支持覆盖外部 let 定义。

## subject 
```ruby
describe Array do
  context '...' do
    it 'is an array' do
      expect(subject).to be_an(Array) # subject隐性指向Array的一个实例
    end
  end
end
```

```ruby
describe SingleDigit do
  subject(:random_digit) { SingleDigit.new } # 显式指定subject（推荐）
  ...
```

## predicate
```ruby
describe Array do
  it 'is empty' do
    expect(subject.empty?).to eq true
  end
  
  it 'is empty' do
    expect(subject).to be_empty 
  end
  
  it { is_expected.to be_empty }
end
# 以上三种写法是一样的
# 在rspec中，对以‘?’结尾的方法（即谓词）的测试，都可以用对应的'be_<how>'形式进行测试
# 特别地，对于'has_<what>?'谓词，对应的测试方法是'have_<what>'
# 对于'include?'，对应的是'include'
```

## truthy & falsy
回顾：在 ruby 中，只有 nil 和 false 为假值。
```ruby
describe '...' do
  it '...' do
    expect(1).to be_truthy
  end
  
  it '...' do
    expect(nil).to be_falsy
  end
end
```

## eq, eql, equal and be
>  ‘*eq*' checks for **equal VALUE**.
>  '*eql*' checks for equal **VALUE and TYPE**.
>  '*equal*' checks for **OBJECT IDENTITY**.
>  '*be*' checks for **OBJECT IDENTITY**. 