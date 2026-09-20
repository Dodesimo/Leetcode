- minimum moves to make array equal:
	- greedy: we always want to increment the n - 1 smallest items
        this is actually equivalent to decrementing the largest item 
        we want to get to a state of the minimum element in the array by decrementing the greatest
        the order in which we do this decrementation doesn't matter for this question we only care about the total number of moves
        so for each item the number of moves is the value of the number - minimum summed
        `(nums[i] - min(nums[i])) += (nums[i + 1] - min(nums[i + 1]))`
        so key insight here: incrementing n - 1 items that are the smallest means keep decrementing by the largest item till everythign is equal to the smallest item
* number of boomerangs:
	* brute force, go through all points, calculate distance count for each with each other
	* then go through again, for each other and calculate the distance, and then add the count - 1 (since we can't double count)
	* better:
		* use combinatorics: ordered pair of k items is k * (k -1)
			* `k(k - 1)` is the number of ways to choose 2 distinct items from `k` items when order matters:
			* P(k,2)=k!/(k−2)!​=k(k−1)
	* so we go through each pointer, calculate distances to other points, and then track how many distances
		* obviously skip yourself
	* and then do this calculation for each distance since we have k items w this distance we want number of ordered pairs
	* return the count
* minimum number of arrows to burst balloon:
	* greedy choice: sort by balloon ending.
		* why? because that limits us
		* if we sorted by start, we could miss a ballon thats at the end that doesn't extend all the way to the right
			* for example:
			* ```
								  If we sort by start point and shoot as far right as possible, we'll shoot too far right for balloon 2:
					
					|       |       (1)
					  |   |         (2)
					      |   |     (3)
					        ^
					
					Sort by end point and shoot as far right as possible and we'll get the correct answer which is two arrows:
					
					  |     |       (2)
					|       |       (1)
					    |   |       (3)
					    ^   ^
			  ```
	* then iterate through starting at index 1, and if the start of the balloon is less than then ending of the current, keep advancing
	* else, increment count, and then update the end, keep advancing
	* then at the end, to account for the last balloon group, we increment the count
	* return count
* artihmetic slices II:
	* hard dp checkout when time