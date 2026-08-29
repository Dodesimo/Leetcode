- rotate function:
	- recurrence pattern to note:
		- ```
			F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
			
			F(1) = (1 * 4) + (2 * 3) + (3 * 2) + (0 * 6) = 0 + 4 + 6 + 6 = 16
			
			F(2) = (2 * 4) + (3 * 3) + (0 * 2) + (1 * 6) = 0 + 6 + 8 + 9 = 23
			
			F(3) = (3 * 4) + (0 * 3) + (1 * 2) + (2 * 6) = 0 + 2 + 12 + 12 = 26
			
			  
			
			F[1] = F[0] + (4 + 3 + 2 + 6) - (len(nums) * nums[n - i])
			
			F[2] = F[1] + (4 + 3 + 2 + 6) - (len(nums) * nums[n - i])
		  ```
		- then implement this recurrence pattern, initialize the past F with f0
- integer replacement:
	- some easy dp
- random pick index:
	- naive easy w/ a list of indices
	- harder: reservoir sampling
		- this doesn't work if the number of calls is the same as the number of elements in the array
		- but the idea is you iterate through the array and maintain track of the number of items we have encountered that equals the target
		- every time we equal the target, we have count items, so if you randomly sample from the count and it equals the count that's a equal probability of updating the index
- evaluate division:
	- using a bfs is easier to get paths from start to end without doing some nonsense
	- create an adjlist have start and end reversed weight
	- start the queue with the start and a weight of 1
		- and then while the queue has items then pop node, value
		- then if the node is end return the value
		- else we add node to the v
		- and then we iterate through all neigh if its in v then we continue
		- else we appendleft(neigh, weight * value)
	- else return -1
	- then iterate through start, end and if neither is in the adj list we append -1
	- else if its equal add 1
	- else append bfs result to result
	- then return result
	- 