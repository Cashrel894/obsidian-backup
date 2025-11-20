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

## predicate
```ruby
describe Array do
  context '...' do
    it 'is an array' do
      expect(subject).to be_an(Array) # subject隐性指向Array的一个实例
    end
  end
end
```