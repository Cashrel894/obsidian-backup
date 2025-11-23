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
place(col, mark) # Place a player's mark('X' or 'O' or custom ones) on a specific column.
display() # Print itself onto the terminal.
```

## Module Referee
- Checks whether the game is over.
- Figures out the winner (or a tie).
```ruby
game_over?() # Checks whether the game is over.
game_result() # Returns the winner of an over game(0 or 1) or :TIE if there's a tie.
```

## Class Game
- Controls the main running logic of the game.
	- Introduce the rules of the game.
	- Circularly requests moves, places marks, checks win conditions, rotates players, and end the cycle with an ending message if the game is finally over.
- Stores all the game states. (e.g., a Board instance)
- Implements a human-computer interface.
	- Prompts and asks the current player for the next move.
		- Reasks if the move is invalid (with an exception message).
	- Announces the end of the game and feedbacks the winner or a tie.
```ruby
@player_markers
@board
@current_player
initialize(player_markers=['X', 'O'], board=nil, current_player=:X) # Creates a game instance with specific player markers, board, and current player.
run() # Executes the main game logics.
rotate_players() # Rotates the current player.
player_input() # Gets a valid move input from the current player.
```