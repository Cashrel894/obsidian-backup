#OdinProject 
## begin... rescue...
```ruby 
begin
  result = 10 / 0
rescue ZeroDivisionError => e
  puts "Error encountered: #{e.message}"
end
```

## ensure
```ruby 
begin
  # Code that might raise an exception
rescue
  # Handle the exception
ensure
  # Code to always execute
end
```

```ruby 
begin
  raise "An error occurred" # 模拟错误
rescue
  puts "Rescue block executed"
ensure
  puts "Ensure block always executed"
end
# Output:
# Rescue block executed
# Ensure block always executed
```

## else 
```ruby 
begin
  # Code that might raise an exception
rescue
  # Handle the exception
else
  # Code to execute if no exception occurs
end
```

## retry
```ruby
attempts = 0

begin
  puts "Trying to execute code..."
  attempts += 1

  # 引发一个异常模拟失败
  raise "An error occurred" if attempts < 3

  puts "Code executed successfully!"
rescue
  puts "Rescue block executed. Retry attempt ##{attempts}."
  retry if attempts < 3 # 仅当尝试次数小于 3 时重试，程序跳转到begin处
end
```
## raise 
```ruby 
raise "An error occurred"

raise ArgumentError, "This is an ArgumentError"
```

```ruby 
begin
  raise "An error"
rescue => e
  puts "Caught exception: #{e.message}"
  raise # 重新抛出异常
end
```

## Custom Error Class 
![[../../附件/Pasted image 20251022172205.png]]
```ruby 
class CustomError < StandardError; end

begin
  raise CustomError, "This is a custom error"
rescue CustomError => e
  puts e.message
end
```