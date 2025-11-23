#OdinProject 
## Class Board
- Stores major game states. 
- Manages player moves.
	- Validates player's choices.
	- Updates game states according to player's choices.
- Displays visible game states.
```ruby
@width
@height
@grid
include Referee
initialize(width=7,height=6) # Create a new board with the specific width and height.
invalid_colomn?(col) # Check if a specfic column is invalid(full or out of range) so that marks can't be placed on it.
place(col, mark) # Place a player's mark('X' or 'O') on a specific column.
display() # Print itself onto the terminal.
```

## Module Referee
- Checks whether the game is over.
- Figures out the winner (or a tie).
```ruby
game_over?() # Checks whether the game is over.
game_result() # Returns the winner of an over game(:X or :O) or :TIE if there's a tie.
```

## Class Game
- Controls the main running logic of the game.
- Stores all the game states. (e.g., a Board instance)
- Implements a human-computer interface.
	- Asks the current player for the next move.
		- Reasks if the move is invalid.
	- Announces the end of the game and feedbacks the winner or a tie.
```ruby

```