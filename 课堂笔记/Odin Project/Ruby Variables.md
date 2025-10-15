#OdinProject 
```ruby
desired_location = "Barcelona"
johns_location = desired_location

desired_location  #=> "Barcelona"
johns_location    #=> "Barcelona"
```

```ruby
johns_location.upcase  #=> "BARCELONA"

desired_location        #=> "Barcelona"
johns_location          #=> "BARCELONA"
```

```ruby
johns_location.upcase!  #=> "BARCELONA"

desired_location        #=> "BARCELONA"
johns_location          #=> "BARCELONA"
```