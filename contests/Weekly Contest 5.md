- find nth digit
	- notice a pattern that the first 9 numbers contribute one digit, 90 numbers contribute two digits, etc.
	- so the N we have, we need to figure out the number digit within the right section
	- for zero indicing and clean modulos, 
	- so while the N is greater than digit * count
		- subtract digit * count
		- increment digit
		- and increment count by ten
	- now N tells us what digit within the digit range
	- if we divide N by the digit, we know what number within, if we mod N by digit we know what digit (since every number in the range has the same number of digits)
		- ```
		  # digit represents the number of digits of the numbers we have
		  # suppose we want the fifteenth digit, falls in the 2 digit section with a offset of
		  # 5. since every number now has two digits, dividing this by two gives us the offset and modding this by the digit tell us the digit in that number
		  ```
	- so divide N by the digit and add by 10 ** (digit - 1) (this gives us the number we care about)
	- and then convert to string the index offset (n % digit), then convert to int
- binary watch:
	- trying to do backtracking on two different arrays, you can create duplicate situations which you don't want
	- create a structured list of lights that combines both hours and minutes
		- this list of lights contains information about whether its hours and minutes
		- maintain a dfs of i, used, hoursSum, minutesSum
			- if the hours sum exceeds 11 or 59, return
			- if the used values lights equal the target truned on, append result and return
		- then get the denom (hour or minute) and value of the light at that index
			-  if its equal to h, add to the hour value and then increment index and used
			- else add to the minutes
		- then skip it by just incrementing i index
		- this ensures there aren't duplicate values created