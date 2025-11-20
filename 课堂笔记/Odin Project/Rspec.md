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

## all & contain_exactly
```ruby
describe Array do
  subject(:numbers) { [11, 17, 21] }

  it 'is all odd numbers' do
    expect(numbers).to all(be_odd)
  end

  it 'is all under 25' do
    expect(numbers).to all(be < 25)
  end

  it 'contains exactly 21, 11, 17' do
    # The order does not matter.
    expect(numbers).to contain_exactly(21, 11, 17)
  end
end
```

## start_with & end_with
```ruby
describe String do
  subject(:sample_word) { 'spaceship' }

  context 'when using start_with' do
    it 'starts with s' do
      expect(sample_word).to start_with('s')
    end

    it 'starts with spa' do
      expect(sample_word).to start_with('spa')
    end

    it 'starts with space' do
      expect(sample_word).to start_with('space')
    end

    it 'starts with the whole word' do
      expect(sample_word).to start_with('spaceship')
    end
  end

  context 'when using end_with' do
    it 'ends with p' do
      expect(sample_word).to end_with('p')
    end

    it 'ends with hip' do
      expect(sample_word).to end_with('hip')
    end

    it 'ends with ship' do
      expect(sample_word).to end_with('ship')
    end

    it 'ends with the whole word' do
      expect(sample_word).to end_with('spaceship')
    end
  end
end
```

## change
```ruby
describe Array do
  subject(:drinks) { %w[coffee tea water] }

  # When testing for a change to occur, notice that unlike previous matchers
  # we've seen, 'change' accepts a block of code.
  # https://rspec.info/features/3-12/rspec-expectations/built-in-matchers/change/

  context 'when testing for a change' do
    it 'will change the length to 4' do
      expect { drinks << 'juice' }.to change { drinks.length }.to(4)
    end

    it 'will change the length from 3 to 4' do
      expect { drinks << 'juice' }.to change { drinks.length }.from(3).to(4)
    end

    # The above two tests are too tightly coupled to a specific array length.
    # The test should instead be written for any length of array, for example:
    it 'will increase the length by one' do
      expect { drinks << 'juice' }.to change { drinks.length }.by(1)
    end

    # There are additional ways to be more descriptive about the change.
    it 'will increase the length by at most one' do
      expect { drinks << 'juice' }.to change { drinks.length }.by_at_most(1)
    end

    # Alternate form for 'change' matcher using (object, :attribute):
    it 'will increase the length by one' do
      expect { drinks << 'juice' }.to change(drinks, :length).by(1)
    end

    # You can compound change matchers together.
    it 'will decrease by one and end with tea' do
      expect { drinks.pop }.to change { drinks.length }.by(-1).and change { drinks.last }.to('tea')
    end
  end
end
```
## custom matcher
```ruby
describe 'defining custom matchers' do
  context 'when reusing a matcher that is in scope' do
    matcher :be_divisible_by_four do
      match { |num| (num % 4).zero? }
    end

    it 'is divisible by 4' do
      expect(12).to be_divisible_by_four
    end

    # You can test for the inverse of the matcher.
    it 'is not divisible by 4' do
      expect(99).not_to be_divisible_by_four
    end

    # You can even use a custom matcher with 'all'.
    it 'works with multiple values' do
      expect([12, 100, 800]).to all(be_divisible_by_four)
    end
  end
end
```