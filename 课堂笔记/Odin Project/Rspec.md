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

## shared_examples
```ruby
include_examples "name"      # include the examples in the current context
it_behaves_like "name"       # include the examples in a nested context
it_should_behave_like "name" # include the examples in a nested context
matching metadata            # include the examples in the current context
```

```ruby 
require "set"

RSpec.shared_examples "a collection" do
  let(:collection) { described_class.new([7, 2, 4]) }

  context "initialized with 3 items" do
    it "says it has three items" do
      expect(collection.size).to eq(3)
    end
  end

  describe "#include?" do
    context "with an item that is in the collection" do
      it "returns true" do
        expect(collection.include?(7)).to be(true)
      end
    end

    context "with an item that is not in the collection" do
      it "returns false" do
        expect(collection.include?(9)).to be(false)
      end
    end
  end
end

RSpec.describe Array do
  it_behaves_like "a collection"
end

RSpec.describe Set do
  it_behaves_like "a collection"
end
```

```ruby
require "set"

RSpec.shared_examples "a collection object" do
  describe "<<" do
    it "adds objects to the end of the collection" do
      collection << 1
      collection << 2
      expect(collection.to_a).to match_array([1, 2])
    end
  end
end

RSpec.describe Array do
  it_behaves_like "a collection object" do
    let(:collection) { Array.new }
  end
end

RSpec.describe Set do
  it_behaves_like "a collection object" do
    let(:collection) { Set.new }
  end
end
```

```ruby
RSpec.shared_examples "a measurable object" do |measurement, measurement_methods|
  measurement_methods.each do |measurement_method|
    it "should return #{measurement} from ##{measurement_method}" do
      expect(subject.send(measurement_method)).to eq(measurement)
    end
  end
end

RSpec.describe Array, "with 3 items" do
  subject { [1, 2, 3] }
  it_should_behave_like "a measurable object", 3, [:size, :length]
end

RSpec.describe String, "of 6 characters" do
  subject { "FooBar" }
  it_should_behave_like "a measurable object", 6, [:size, :length]
end

```

## allow & receive
可以使用 allow 和 receive 来允许使用指定返回值模拟方法调用，而不实际调用方法。
```ruby
describe NumberGame do
  describe '#player_turn' do
  # In order to test the behavior of #player_turn, we need to use a method
  # stub for #player_input to return a valid_input ('3'). To stub a method,
  # we 'allow' the test subject (game_loop) to receive the :method_name
  # and to return a specific value.
  # https://rspec.info/features/3-12/rspec-mocks/basics/allowing-messages/
  # http://testing-for-beginners.rubymonstas.org/test_doubles.html
  # https://edpackard.medium.com/the-no-problemo-basic-guide-to-doubles-and-stubs-in-rspec-1af3e13b158

  subject(:game_loop) { described_class.new }

  context 'when user input is valid' do
    # To test the behavior, we want to test that the loop stops before the
    # puts 'Input error!' line. In order to test that this method is not
    # called, we use a message expectation.
    # https://rspec.info/features/3-12/rspec-mocks/

    it 'stops loop and does not display error message' do
      valid_input = '3'
      allow(game_loop).to receive(:player_input).and_return(valid_input)
      # To use a message expectation, move 'Assert' before 'Act'.
      expect(game_loop).not_to receive(:puts).with('Input error!')
      game_loop.player_turn
    end
  end
  
  context 'when user inputs an incorrect value once, then a valid input' do
      # As the 'Arrange' step for tests grows, you can use a before hook to
      # separate the test from the set-up.
      # https://rspec.info/features/3-12/rspec-core/hooks/before-and-after-hooks/
      # https://www.tutorialspoint.com/rspec/rspec_hooks.htm

      before do
        # A method stub can be called multiple times and return different values.
        # https://rspec.info/features/3-12/rspec-mocks/configuring-responses/returning-a-value/
        # This method stub for :player_input will return the invalid 'letter' input,
        # then it will return the 'valid_input'
        letter = 'd'
        valid_input = '8'
        allow(game_loop).to receive(:player_input).and_return(letter, valid_input)
      end

      # When using message expectations, you can specify how many times you
      # expect the message to be received.
      # https://rspec.info/features/3-12/rspec-mocks/setting-constraints/receive-counts/
      it 'completes loop and displays error message once' do
        expect(game_loop).to receive(:puts).with('Input error!').once
        game_loop.player_turn
      end
    end
  end
end
```

## double
```ruby
describe FindNumber do
  # There are two ways to specify the messages that a double can receive.

  describe '#initialize' do
    # Doubles are strict, which means you must specify (allow) any messages
    # that they can receive. If a double receives a message that has not been
    # allowed, it will trigger an error.

    # This first example creates the double, then allows specific methods.
    context 'when creating the double and allowing method(s) in two steps' do
      let(:random_number) { double('random_number') }
      subject(:game) { described_class.new(0, 9, random_number) }

      context 'when random_number is 8' do
        it 'returns 8' do
          allow(random_number).to receive(:value).and_return(8)
          solution = game.answer
          expect(solution).to eq(8)
        end
      end
    end

    # This second example creates the double & specific methods together.
    context 'when creating the double and allowing method(s) in one step' do
      # A hash can be used to define allowed messages and return values.
      # When passing a hash as the last argument, the { } are not required.
      # let(:random_number) { double('random_number', { value: 3 }) }
      let(:random_number) { double('random_number', value: 3) }
      subject(:game) { described_class.new(0, 9, random_number) }

      context 'when random_number is 3' do
        it 'returns 3' do
          solution = game.answer
          expect(solution).to eq(3)
        end
      end
    end

    # When testing the same method in multiple tests, it is possible to add
    # method names to subject.
    context 'when adding method names to subject for multiple tests' do
      let(:random_number) { double('random_number', value: 5) }
      subject(:game_solution) { described_class.new(0, 9, random_number).answer }

      context 'when random_number is 5' do
        it 'returns 5' do
          expect(game_solution).to eq(5)
        end
      end
    end
  end
end
```

5