- fenwick tree:
	- allows to do prefix queries quickly
	- `O(nlogn)` to construct lazy, `O(log n)` to do retrieval operations and update
	- we maintain a tree of n + 1 indices, where each index has a parent based on removing the last set bit
	- to get a particular range query, go to the number node after the ending and then go up the parents 
		- so get parent w/ 2's complement, AND with original number, then subtract from original number
		- unset last bit, go the parent.
		- keep doing this till you reached the root
		- this means keep going up earlier disjoin ranges used in a prefix query
	- one indexed, so we use terminology of query first N elements
		- so for a range of index 0 to 2, since second index corresponds to the first three elements, we use query(3) - query(0)
		- `query[r + 1] - query[l]`
	- then go to the number node as the start, and then go to the parent and then subtract that 
	- to populate:
		- we have an array
		- in this array, the parents of children have their last set bit removed
		- so we start an an array, then traverse indices in bound by adding the lowest set bit (i + (i & -i))
		- an optimization is that we only send it to the direct child (the next value recieved by adding the lowest set bit)
		- this is because we send already-combined partial sums
			- the contribution of earlier indices get contributed to later indices through middle indices
	- to search, move towards 0 by subtracting the last set bit
		- this removes the lowest set bit
		- and just sum
	- this is actually represented in array
	- each index contains the sum of `[i - lowbit + 1, i]`
	- to update, keep adding the lowest set bit to move to the next broader index 
- segment tree:
	- allows for min max range queries (not prefix)
	- essentially  we have a tree where the root is the entire array w/ the calculated, value, and we keep splitting the array in half till the leaves are individual elements (each having a interval of `[i, i])
	- to calculate a range query:
		- at each node, compare interval w/ requested interval
		- total overlap:
			- if the node interval is completely within the query interval, return value stored there
		- partial overlap
			- evaluate both subtrees
		- zero overlap
			- return a sentinel value
		- for updates:
			- we only have to update intervals that contain it

code:
- fenwick tree:
	- ```python
	  class FenwickTree:
		def __init__(self, nums):
			  self.n = len(nums)
			  self.tree = [0] * (self.n + 1) #one more than the original size
			  for i, value in enumerate(nums):
				  self.update(i, value)
		def update(self, index, delta):
			i = index + 1 # because the fenwick tree is 1 based
			while i <= self.n:
				self.tree[i] += delta
				i += (i & -i) # we move to the next value in the array that is a larger index
		def query(self, i):
			# we get the first i elements sum
			total = 0
			while i > 0:
				total += self.tree[i]
				i -= i & -i
			return total
		
		def range_query(self, left, right):
			return self.query(right + 1) - self.query(left)
	  ```
	- `O(n)` construction and then `O(log n)`
- segment tree:
	- ```python
	  class SegmentTree:
		  def __init__(self, nums):
			  self.n = len(nums)
			  self.tree = [0] * (4 * self.n) # safe amount because if its not a exact multiple of two
			  if self.n > 0:
				  self._build(nums, node = 1, left = 0, right = self.n - 1)
		  def _build(self. nums, node, left, right):
			  # leaf node
			  if left == right:
				  self.tree[node] = nums[left]
				  return
			  mid = (left + right) // 2
			  self._build(nums, node * 2, left, mid) # build the left subtree
			  self._build(nums, node*2 + 1, mid + 1, right) #build the right subtree
			  self.tree[node] = max(self.tree[node*2], self.tree[node*2 + 1])
		
		def update(self,index, value):
			self._update(node = 1, left = 0, right = self.n - 1, index = index, value = value)
		
		def _update(self, node, left, right, index, value):
			if left == right:
				self.tree[node] = value
				return
			mid = (left + right) // 2
			if index <= mid:
				self._update(node * 2, left, mid, index, value)
			else:
				self._update(node*2 + 1, mid + 1, right, index, value)
			# then we recalculate node after update
			self.tree[node] = max(self.tree[node * 2], self.tree[node * 2 + 1])
		def query(self, query_left, query_right):
			return self._query(node = 1, left = 0, right = self.n - 1, query_left = query_left, query_right = query-right)
		
		def _query(self, node, left, right, query_left, query_right):
			# no overlap we return a sentinel value
			if right < query_left or left > query_right:
				return 0
			if query_left <= left and right <= query_right: # complete overlap
				return self.tree[node]
			mid = (left + right) // 2
			leftMax = self._query(node * 2, left, mid, query_left, query_right)
			rightMax = self._query(node * 2 + 1, mid + 1, right, query_left, query_right)
			return max(leftMax, rightMax)
	  ```
	- we need this because range queries and point updates are `O(log n)` and for normal array, range sum query is `O(n)` and point updates are `O(1)` and for prefix sum, the range sum queries is `O(1)`and point updates is `O(n)`
