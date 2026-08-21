
- when its not infinite
	- ```python
	  class ConnectFour:
		  ROWS = 6
		  COLS = 7
		  CONNECT = 4
		  def __init__(self):
			  self.board = [[0] * self.COLS for i in range(self.ROWS)]
			  self.nextRow = [self.ROWS - 1] * self.COLS # for each available column index, give me the immediately available row to place the item
		 
		 def move(self, col, player):
			 if col < 0 or col >= self.COLS or player not in (1, 2):
				 return -1
			 row = self.nextRow[col]
			 if row == -1:
				 return -1
			 self.nextRow[col] -= 1
			 self.board[row][col] = player
			 if self._winning_move(row, col, player):
				 return player
			 return 0
		
		def _winning_move(self, row, col, player):
			directions = [[0, 1], [1, 0], [1, 1], [-1, 1]]
			for dr, dc in directions:
				count = 1
				count += self._getTotal(row, col, player, dr, dc) # go in one direction
				count += self._getTotal(row, col, player, -dr, -dc)
				if count >= self.CONNECT:
					return True
			return False
		
		def _getTotal(self, row, col, player, dr, dc):
			r = row + dr
			c = col + dc
			count = 0
			while 0 <= r < self.ROWS and 0 <= c < self.COLS and self.board[r][c] == player:
				count += 1
				r += dr
				c += dc
			return count
			
	  ```

- infinite
	- ```python
	  import defaultdict
	  class InfiniteConnectFour:
		  CONNECT = 4
		  def __init__(self):
			  self.board = defaultdict(int) # stores (row, col) pairs
			  self.nextRow = defaultdict(int) # stores next available row for each column
		  def move(self, col, player):
			  if player not in (1, 2):
				  return -1
			  row = self.nextRow[col]
			  self.board[(row, col)] = player
			  self.nextRow[col] += 1
			  if self._winning_move(row, col, player):
				  return player
			  return 0
		 
		   def _winning_move(self, row, col, player):
			   directions = [[0, 1], [1, 0], [1, 1], [-1, 1]]
			   for dr, dc in directions:
				   count = 1 
				   count += countTotal(row, col, player, dr, dc) #go in one direction
				   count += countTotal(row, col, player, -dr, -dc) #go in the other direction 
				   if count >= CONNECT:
					   return True
				return False
				
			def countTotal(self, row, col, player, dr, dc):
				row += dr
				col += dc
				count = 0
				while self.count < CONNECT - 1 and self.board[(r, c)] == player:
					count += 1
					row += dr
					col += dc
				return count
			
	  ```
- 