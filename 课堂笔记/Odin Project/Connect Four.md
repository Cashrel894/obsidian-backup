#OdinProject 
## Class Board
- Stores major game states. 
- Manages player moves.
	- Validates player's choices.
	- Updates game states according to player's choices.
```ruby
@width # The number of columns.
@height # The number of cells that a column contains.
@grid # a 2D array, the first dimension stores all the columns, and the second stores the cells in a certain column. nil for an empty cell, 0 for the first player, 1 for the second player.
include Referee
initialize(width = 7, height = 6, grid = nil) # Create an board with the specific width, height and grid.
invalid_column?(col) # Check if a specfic column is invalid(full or out of range) so that marks can't be placed on it.
place(col, mark) # Place a player's mark on a valid column.
```

## Module Referee
- Checks whether the game is over.
- Figures out the winner (or a tie).
```ruby
game_over?() # Checks whether the game is over.
tie?() # Checks whether there's a tie.
winner() # Returns the winner(0 or 1) of an over game without a tie.
```

## Class Game
- Controls the main running logic of the game.
	- Introduce the rules of the game.
	- Circularly requests moves, places marks, checks win conditions, rotates players, and end the cycle with an ending message if the game is finally over.
- Stores all the game states. (e.g., a Board instance)
- Implements a human-computer interface.
	- Shows the current board state.
	- Prompts and asks the current player for the next move.
		- Reasks if the move is invalid (with an exception message).
	- Announces the end of the game and feedbacks the winner or a tie.
```ruby
@player_markers
@board
@current_player
initialize(player_markers = ['X', 'O'], board = nil, current_player = 0) # Creates a game instance with specific player markers, board, and current player.
run() # Executes the main game logics.
rotate_players() # Rotates the current player.
player_input() # Gets a valid move input from the current player.
display_board() # Print the board state onto the terminal.
```