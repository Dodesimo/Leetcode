### General tips:
- When wanting key overwrites, [],
- [] default constructs a value and inserts it.
- When not wanting key overwites, append().
- Emplace_back is an optimization for very large/expensive things to copy.
- Remember that references are bound.
- Arrays are not hashable in c++

### Design hashset and hashmap:
- Remember to only add when the key isn’t present.
- Hash to certain buckets and add it to that.

### Decode and encode string:
- To get the digit value of a single character, subtract 0 from it.
- Use a std::stringstream to not worry about any sort of formatting.

### Range sum 2d:
- Have a row of padding at the top, have a col of padding to the left (0) for sum.
- Idea of precomputation, product of everything from (0,0) to (i, j).
- Idea: look at the row before row 1, the column before col 2, and then subtract from row2, col2, then readd row1 - 1, col1 -1 (double counted). With the padding, we don’t have to worry about edge cases.

### Majority Elemetn II:
- For all majority element questions, consider boyer moore algorithm for voting.
- Handle both the majority element (increase its frequency), then see if the votes are 0, if so consider the current element as that majority element and increase frequency. Else, nothing matches so we decrement the votes.
- Then, we want to validate that the frequency is actually n / 3, and then we add the first element if so. We add the second element if its also and its not equal to the first element.

### Subarrays Sum == K:
- If we keep a running sum, to get K, we want to find the number of times a “prev” sum occurred such that prev + k = running sum.
- So we maintain a base case of sum of 0 is 1 (for a sum of 0, only one possible array).
- Increment the sum by the element, and then subtract from k to get that previous. This is because the sum considers the number of subarrays that can be formed at that particular element summing to k.
- We then add that to a total, then increment the number of subarrays with the current sum.

### First Missing Positive:
- Observation that within the array, only things between 1 to nums.size() could be missing.
- Use the array itself to hash results. (unoptimized way, hash everything, and get the minimum and the gap). If the minimum is 1, we use the gap, else 1)
- We iterate through and set <= 0 to nums.size() + 1 (no way this can ever be the answer)
- We then iterate through the array, and if the number falls within the range 1 to nums.size() inclusive, we mark the value - 1 index with a negative sign.
- Tells us value was present.
- We then iterate through the array and find the first positive value as that tells us that there wasn’t something with a negative.
- We then return index + 1.
- This works in the sitaution because if we have a continuous range thats from first element onward, doing this gets us a minimum of 1 that’s possible.
- At the end, if we couldn’t find a positive, we return nums.size() + 1.

### Valid Palindrome:
- To make an entire string lower case, we use std::transform, passing in the beginning iterator and the end iterator, and then a lambda function that takes an unsigned char and then does std::tolower, capture clause empty.

### Merge Sorted Array:
- Easy with extra space.
- You can work backwards and then add the elements, and then if our first pointer is exhausted, we don’t do anything cuz its already populated.
- However, if the second is not overwritten the values.

### Remove Duplicate:
- First index always has a unique value.
- Set the replace pointer to the index 1.
- Set then iterate from 1 to the end, if we see that a value is not the same as the previous value, set it to the pointer, increment ht epointer.
- THIS IS THE SAME LOGIC AS REMOVE ELEMENT.
- Have a pointer variable for removal, go through the array, use an if statement to test for a condition, if it fulfills, put it at that pointer, then increment the pointer.

### Three Sum:
- Remember to move both pointers.

### Four Sum:
- Same idea as three sum, but we need to check for duplicates at each level. After we consider something for the second value, we just need to see whether this isn’t repeated.
- Hence why we do j > i + 1 (we want to consider the first value), but not the later ones.

### Rotate Array:
- If our k is fully divisible, just return.
- With space: easy, mod copy.
- Else, we reverse the array, and then we do reversal of the first k items, and then reverse the rest of it.
- For in place manipulations, we should use reversals and other techniques to identify optimal ways to solve the question.

### Container of Most Water:
- The smaller column is the bottle neck so lets use that to restrain.

### Boats to save people:
- Sort, we can pair heaviest with lightest and exceeds limit> lets send just the heaviest with the boat, decrement.
- Else, lets put both of them in a boat and consider the next pair.
- Why this works even if we are less than capacity?
- Because if the heaviest can fit with the lightest we have so far, we are giving them the best possible choice because there’s a chance that the one that’s slightly heavier than the lightest one doesn’t fit.

### Trapping Rain Water:
- Only update the left and right for a leftMax and Rightmax Being left becaus we don’t know for the other if there’s going to be a lesser one.
- If we know left max is less than right max, then for s[l], we know that left is the smallest possible value to consider.
- For r, there could be something after the l pointer thats smaller, so we shouldn’t update.

### Baseball Game:
- You can calculate the sum at once, just use a stack and update the total that way (you don't have to do a second pass).

### Valid Parenthesis:
- You can value initialize a dictionary with { {}, {}, {} }, where nested parenthesis represent key value pairs separated by commas.

### Implement Stack with Queues:
- I had the right idea.
- We want to have two queues, and when doing push, we push to the second queue, then get everything from the first queue over to the second queue, and swap the two queues such that q1 is always holding the correctly ordered queue.
- Then the rest of the operations are easy.
- We can also use a single queue and then essentially rotate it through pops n - 1 times till the current element appears at the top.

### Implement Queue with Stacks:
- Can’t do previous question approach because popping from a stack and reading doesn’t do anything since ur altering the literal same place.
- Have a second stack that represents the front of the queue. We only reverse the firs stack and put in the second stack when its empty because the top of the second stack represents the top of the queue.
- Brute force: we would always reverse the contents into the second stack and then return the top, but in this situation we are efificnet and only move stuff over to the second queue once there’s nothing there.

### Min Stack:
- Nothing to say, makes sense.

### Evaluate Reverse Polish Notation:
- Std::stoi to convert a string into numerical value.
- Remember to do first and second in order.

### Asteroid Collision:
- A collision can only happen if there’s an asteroid moving to the right and then one that’s moving to the left.
- Remember what happens in all three cases and what gets popped.
- For the particular asteroid we are iterating, have a boolean flag dictating if its alive or dead based on magnitude of what's top of the stack.
- Obviously if top is equal to what we have, then its dead and we fade.
- If the top is greater than what we have, also dead and we fade.
- Else we continue popping from loop.
- Then at the end if its dead, we don’t add else we do.

### Daily Temperatures:
- Mono stack, think about it.
- Create a std pair stack, and then autofill constructor for output that has (size, value), creates number size copies of values.

### Stock Span:
- Question boiled down to what is the first greatest value on the left of a current one.
- So if we maintain a stack where once we found a greater value, popped all smaller ones such that we had the greatest top value, we can then get a difference in index between the two and that becomes our span.
- Use an index value to keep track of the index of the value we would have if this was an array.

### Car Fleet:
- Sort position in decreasing order.
- Calculate time needed in a double.
- Initialize a stack with the last item.
- Stack always keeps the last item for a merge.
- So if we have a greater time, we push it onto the stack and then we increment fleets.

### Simplify Path:
- Delimit with a token std::string, initialize the stringstream with the actual string, then do whilegetline with the token delimiter and then do processing, if == “”..” pop stack if “.” or the size of what we have is 0 case with multiple ///, then continue. Else push. Then at the end initialize a string output with “/”, then keep adding till the last item, then add it to the stream, then return .str().

### Decode String:
- We essentially keep adding stuff to a stack until we see a closing one.
- When we see a closing one, we gather the string collected so far, then reverse it.
- We then pop because we know it will be an opening bracket.
- We then collect the number, realizing that its stored least significant digit first.
- We then gather the number, and then from there we insert the string so far that many times into the stack.
- Convert the stack to a string by invoking std::string constructor that takes iterator begin for the stack and then iterator end,

### Max Frequency Stack:
- Keep track of frequency and create a map of stacks that corresponds each frequency to its stack.
- We increase an item's frequency and then we update frequency with the max of frequency itself and the frequency we just increment.
- Then we append the item to the vector for that frequency.
- To pop, we then get the back of the max frequency and then pop it, decrease its frequency, and then see whether the max frequency is now empty.
- If so, we then consider the next highest frequency by decreasing by one.
- The reason why this works is because mapping is continuous.

### Largest Rectangle In Histogram:
- Forward pass, we essentially want to see what rectangles we can carry to the right.
- Once we reach a smaller rectangle, for the larger rectangle, we calculate its possible height as we extend it to hte right.
- Then we push the smaller rectangle to the index of hte larger one because that is the farthest it can be expanded to the left.
- Then, we look at everything that was as far to the left possibly exxpanded and calculated max area based off of that.

### Insertion Point:
- Left is like whats greater so u wanna add there.

### Sqrt(x):
- Round down so since r is less than l, because we know if we dont find an exact match, its between l and r we round down,

### Koko Eating Banans:
- Get time needed.
- If our time is greater than the target, we need to decrease our rate (which is between 1 and the max).
- Else, we increase our rate if time less than target, but we keep this in mind as the smallest possible so far.
- To get the number of hours for a particular tae, we math.ceil the division of of pile by rate.
- If we are taking too

### 1011. Capacity To Ship Packages Within D Days
- To get the number of days, do running counts of sums till capacity.
- Start with days 1, when we overlfow, new day, start the sum with the current.
- When doing the binary search think about the ranges of capacities, so imininm would be we can just take all items individually (so capacity would be max of array), maximum would be the whole sum of the array.
- Binary search is the same as over a range.
- If the day exceeds, we need to increase our rate.
- If the day decreases, see if we can get a smaller rate.

### Find MInimum in Rotated Array:
- Just spot an invariant, convert into trues and falses, we need to find the first value fulfilling an invariant.

### Search in Rotated Array:
- Only one half of this array is sorted, we need to find that half and move the search there if the value is within that range.

### Search in Rotated Array II:
- Ssame idea as first, but we run into an issue where the left mid and right are all equal.
- In a situation like that we don’t know where to go, so we should move left, right and recalculate our midpoint.
- Before this, we should directly check whether the m contains the target.
- Else we do the recalculation and then  the rest of the normal check for search in rotated array.

### Sorted Time Store Key:
- Use binary search
- Edge case where if the timestamp is less than the first item timestamp, no match.
- Account for this.
- Ordered map of string to vector of pair of items.

### Split Array Largest Sum:
- Idea is that we essentially binary search across the range of potential subarray sums.
- If we are able to split <= k, then we return true that a split is possible.
- Why less than equal to? Because if we have less than k subarrays, we can maintain k subarrays by splitting further and by doing so we only make the max subarray sum less.
- Which is acceptable because what are trying to check is if its possible less than equal to a particular value because we are considering the smallest possible sum.
- We then binary search from the largest item in the array and then up to the sum.
- If a particular split is possible, we see if we can use a smaller value, r = m - 1, min out.
- On the other hand, if not possible, we have too many components, so lets increase the sum, l = m + 1.

### Median of Two Sorted Arrays:
- Fade this question
- Binary search smaller array (A)
- Have a left max that’s the partition - 1, and a right min thats the partition.
- Do the search inclusive of the two bounds to ensure that all edge cases handled.
- We know we have correct order if leftMax of each is smaller than right min of the other.
- Else, if the left max of A is larger than the right min of B, move to the left.
- Else move to the right.

### Mountain in Array:
- This was on my bloomberg interview
- We need to find the peak, we do this by essentially moving towards the max element in the array.
- Checking if m + 1 is greater than m is valid here because under an invariant the peak will never be at the edge of array, do it anyway.
- We then need to binary search the range from 0 to peak, and then from peak to end.
- Very straightforward.

### Contains Duplicate II:
- Use a hashmap that tracks indices, if the current index and what we have seen so far for a number is the same

### Buy or Sell stock:
- Straight forward, if the profit is negative we try something else by moving l to r and then incrmeenting r. Else, we max out our current result.

### Longest Substring without Repeating Characters:
- Keep a frequency map of each character’s count.
- Left and right pointers
- Increase the frequency of the right character.
- Get the max frequency of the substring so far.
- Then we know that we can have at most (r  - l + 1 - maxFrequency) characters left.
- So then we try to create this invariant by popping left characters and updating frequencies and the max.
- Then we update the max.
- Then we increment r.
- We then return result.

### Permutation in String:
- Really straight froward problem, rmemeber thatn left is inclusive, right is exclusive, so after ur last while loop iteration, u needa explicitly check the last subwindow.
- Don’t use reserve 26, that doesn’t actually create the 26 entries in the table.
- Instead, use fill constructor of (items, value).

### Minimum Size Subarray Sum:
- Got the right idea.
- Just keep adding on the right and incrementing R pointer.
- Be mindful of the sliding window size (its not always just r - l + 1, in this case it would be r - l).
- Be careful where the invariant is.
- While we add doesn’t necessarily mean that this is greater than the target, so only update the target when we know for sure the invariant is correct.

### Closest K Arrays:
- We binary search for a starting position.
- So we have 0, n - k - 1.
- Calculate a midpoint, and then if this element as a start of is greater in diff than element m + k away, we move search to right.
- Else, we move our search to the left because this is a candidate solution but we want to further to the left and we could do so by taking minimum with a candidate and then keep going to the left.
- Then we just add l and l + k to our result.

### Minimum Window Substring:
- Get a frequency of t.
- Have a need and have variable.

### Expand like the following:

### Process:
- Increase frequency of s[r]
- If this is in t’s frequency and its frequency in s matches frequency in t, increase have.
- Contract;
- Here we contract to see if we are still valid.
- So we do something with the window within the contract.

### While have == need:
- Update the max size and result variable
- Reduce frequency of l.
- After this is the frequency in s is less than corresponding frequency t, reduce have.

### Expand:
- Increase r pointer.
- in max situations, we contract the window and then process the window. in min situations, we process the window as we contract for minimization purposes

### Sliding Window maximum:

### Dequeu has two purposes:
- Queue to enforce order
- Stack: to push maximum
- We push indices on it such that the stack portion is strictly decreasing. This makes the front increasing.
- We increment l
- Additionally, inside the loop, we invalidate indices we don’t care about by getting rid of them from the front if the are smaller than l.
- Then we get rid of entries that are smaller than the r value then we insert the r value from the back.
- We then insert the front value of the queue because we know its the largest value within the window.
- Key realization: use indices, the deque can have both stack and queue behavior.
- Static sliding window questions; do a processing in the beginning of the loop after building the sliding window.
- Then we have to do one process out of the loop because r pointer exceeded bound.

### Reverse A Linked List:
- Given to u pointers so use -> to access attributes
- Use nullptr to initialize values or u can just value initialize.
- Conditions see if whether a particular node is nullptr.

### Merge Two Sorted Lists:
- Be careflu about whats a pointer and whats not.
- Other than that use the -> operator to access fields of the ListNode.

- Linked list Cycle;
- Explicitly check cases of null or one pointer.
- Make sure that slow and fast differ by one.
- Then check actual node value then move slow by one,  then fast by next to next.
- at the enda after the loop return false.

### Remove Nth Node from End of List:
- Attach a dummy node so you don’t have to worry about edge cases like [1, 2], n = 2.
- We will always be in bounds in terms of operations.
- Move fast pointer n times.
- Then move the slow pointer starting from ehad till we get out bounds.
- Keep track of your previous
- Then delete node by setting previous next to current.next

### Copy List with Random Pointer:
- If we are doing new object creation within a loop, the object will be destroyed when we leave the loop, so use heap memory.

### Add Two Numbers:
- Heap allocation to return this back to the caller.
- Caller will take care of the deletion.
- Make sure we due remainder and division properly.

### Find the Duplicate Number:
- Floyd’s hare/turtle algorithm
- Linked list where value points to an index.
- Slow and fast pointers. When they intersect, we know that if we start a slow pointer we then have a slower pointer, we then move them both forward till they are equal, that’s where the loop started.

### Reverse Linked List II:
- 1-indexed, so maintain a previous pointer till we hit the left.
- Then, reverse the list from left to right such that the left node has a new previous of null.
- From there, previousPointer.next.next is current (because that’s the new right pointer).
- Also, previousPointer.next = previous because that’s the new “head” of this part of the linked list.
- We have an edge case of where previousPointer can be null. This is when we start at the first node.
- If we attach dummy node, we can use the same logic.
- Use a dummy node whenever there’s a chance of first node getting altered.

### Design Circular queue:
- Don’t accidentally design a stack, what we enqueue at the end, we dequeue at the front.
- Linked list (we don’t need double ended since we aren’t removing from back).
- Have two variables, a capacity and size variable.
- Enqueue nodes, we just add to the back remember, edge case of the list being empty ( can easily check by seeing whether left and right are null)
- Dequeue nodes, we just delete the current node, and then move head to the next node.
- Or we can just move the head pointer to the next one if we still want that in memory, but you can just delete.
- For capacity and size, just see whether the size/cap values are greater than zero straight forward.

### LRU Cache:
- List node needs to contain key, value, previous, next.
- Within our main class, we define a start pointer and a end pointer.
- These point next to each other, and then the capacity is set.
- Define remove function that removes a particular node taken in.
- Add function: adds to end.
- These functions are a not easier to implement if we just deal with pointers.
- Get, we see if present, if not, return -1.
- Else, we remove and add. Then return the value.

### Add:
- If not present, we make  new node, set the key and value, and then add it to hashmap.
- Else, we add remove and then reset the value.
- If we have exceeded the capacity, left.next is the thing we have to remove. So we remove it, and then using the key stored in the class, we delete from hashmap using erase function.

### Things to consider later:
- Add a destructor to LRU cache so that all existing nodes get destroyed correctly.
- std;:Unique pointer.
- Issue: unique pointer can’t be copied.
- So would have to do move semantics.
- Stick with raw pointers.

### LFU Cache:
- Devious
- Created linked list with left and right pointer and a key.
- Create a LRU linked list with the same methods, remove left, add right, remove key, as well as length.

### Within our main function:
- Have the capacity, least frequency, a key to value map, a key to frequency map, and a frequency to LRU Linked list map.
- Have a counter updater for a key.
- In this, we look at the previous frequency, we reduce the frequency of the current key, then we pop key from current LRU frequency.
- If the next level doesn’t have a linked list yet, init a linked list for it, then add the current node there.
- We need to update our leastFrequent.
- If we no longer have a node in that frequency and the previous frequency matched that node, we update least frequency to the new one.
- Get, we check whether the current key is even present. If not return, else, we update the count in the counter update logic, and then return the value for the key.
- For put, we first see whether our capacity of LFU is 0. If so, return immediately.
- Else, we need to see whether this key is new, if so and the capacity is met, remove the key from the corresponding linked list, and then delete from frequency map and key value map.
- Then, we add key and value.
- We then check whether this key has been seen before.
- Check frequency count.
- If it's not, add it to the 0 list.
- Then we update the frequency of the key, and set frequency to 0.
- Why 0? We take care of update logic in the loop so we know its going to be incremented to 1.
- Then we update.
- SUPER UNITUITIVE, REVIEW>

### Merge K Linked Lists:
- Remember pop_back does not return the item.
- So we need to use back in order to get the last item because we pop it.
- We do a log wise merge. Rather than merge one and then keep merging with the rest (that’s O(n * k), n is total number of nodes/some proportion, k is the total number of linked lists).
- Log merge, merge two, then we have a layer of merged lists.
- Merge those.
- Remember to check for null condition for the second pop.
- Also check whether the vector is empty or not.
- When doing the merge method for the two.
- Remember to move appropriate pointers + have a dummy node.

### Reverse Ndoes in K Group:
- Use a reverse helper function that reverses exclusive of the last node.
- Then within the main function, use a dummy node to point to head.
- Initialize start and previous and move end pointer by k > 1 (because we are counting hops). During this, if end ever gets null, that means we don’t have a full k nodes to reverse, so we will break and return dummy->next.
- At the end of hte loop we still have that, so handle that case.
- Store the next node
- Then reverse from start to end->next, we know that end is now the new head start is the new tail
- Adjust pointers, previous next is the new start, star is new tail and thus next is the new next node.
- Then, our previous is going to be start
- Start is going to be start.next;

### Diameter of Tree:
- Just find max on left and right and sum that.
- Keep track of this in a global variable.

### LCA of Tree:
- Just remember to return recursive calls.

### Level Order Treavrsal of Tree:
- remember that deque pop back doesn’t return a value, so use back to get it first.

### Insert into Binary Search Tree:
- Use pointer reinforcement where we reconnect tree nodes.
- when we reach a null spot, return the node.
- then dependent on which way to go to, set left or right node.
- then at the end, return root.

### Delete Node in Binary Tree:

### To delete we have three cases:
- If the node has no children, then we return null
- If the node has one child we return that particular node
- If the node has two children, we get the successor of the node (go to the right, then go all the way left), and then we put that value in the place of the node’s value.
- Then we delete the successor
- If node is null, return null
- If the node currently is less than key, go to the right node->right  is the deletion in the right with that value.
- If the nod eis currently greater than key, go to the left, node->left is the deletion in the left with that value.
- Else we found the node, if no children (both pointers are null), return null, if one child, return that, else find successor through successor function that goes to the right and then goes to the left, then copy that value in the value of the current node, and then we do deletion where node.right = deletion of the successor (we know this works because node.right houses the successor).
- Else, return the main node.
- So essentially pointer reinforces says that we reconnect nodes based on the recursive stack and parent pointers.
- We reconstruct left and right based on the deletion since every call returns a node, which connects to a parent’s pointers.

### Right View of Binary Tree:
- Remember that we need the right most node instead of value because there’s ambiguity if the node’s value itself is 0.

### Construct Quad Tree:
- Complex, but just use column and row bounds to do processing.
- Naive
- Check all nodes of a grid with whether its 0 or 1, and then based on that if it is, follow instructions.
- If not, check the topLeft, topRight, bottomLeft, bottomRight based on exclusive bounds (so the end column, start column, end row, start row).
- If in the middle, remember to average the start and end row/column.
- then return that linked.

### Optimized:
- We don’t want to do a N^2 scan for every matrix.
- Base case, we just have one item (so because it's exclusive its colStart == colEnd - 1, so on), follow instruction
- calculate all the quadrants, see if they are all leaves, then if they all equal same value and leaves, just return a larger Node as a leaf of all of them combined.
- Else, they are different and the node itself is not a leaf, so attach the topLeft, topRight, bottomLeft, bottomRight to the central node.
- return that node.

### Count Good Nodes in Binary Tree:
- Question devolves into whether node is greater than everything in path.
- If we hit end (nullptr), return 0.
- then if the current value is greater than the max, we will recurse on the left and the right with this updated since its the root, and then add 1 to that result.
- else, we recurse left and right normally, then add those.
- return.

### Validate BST:
- We have two bounds that we check, >= the right most, <= the left most.
- if so return false.
- we then go to the left subtree, with the upper bound of the current node’s value, we then go to the right subtree with lower bound of the current node’s value.
- if our root becomes null, this means we haven’t had a false yet so we return true since everything down this path is correct.
- since input can be up to 32 bits, use long instead of int.
- int caps out at 16 bits.

### Kth Smallest:
- Use a preorder traversal, can do this in a separate array and then return that.
- But we can keep count of nodes and then subtract.
- No need to return anything, we just do dfs on the left, subtract our k if its equal to 0 then we set answer node to that value and we return.
- This is efficient since O(n)
- Since we want to have a shared k and answer across all calls, use references since local copies will be destroyed throughout the process.
- or we pack everything together in a vector and pass it to the function.

### Make Binary Tree from Preorder and INorder:
- Did brute force where we have the root be first item in preorder, and everything to the left is the left subtree in inorder, and everything to the right is the is the right subtree in in order.
- So we scan the inorder array every single time and then right subtree starts from the i + 1 + target - l
- left from i + 1
- Inefficient
- Have a limit based approach.
- Maintain global preorder and inorder counters.
- When preorder exceeds the size of preorder array, return null.
- if inorder array value equals our limit, increment inorder and then return nullptr.
- make node from preorder value then increment, then pass in current node value as limit for left, then maintain existing limit value as limit for the right then return the node.

### House Robber Three:
- Maintain a pair of robbing w/ root and without robbing root.
- if we have a null node, return (0, 0)
- get left recursive
- get right recursive
- w/ root means we can’t use the roots for the left and the right
- w/o root means we can use either for the left and right
- then return
- call this in main and max out each.

### Delete All Leaf Nodes:

### My approach:
- If root is null return null
- If leaf and equal value return nullptr (ideally we delete this)
- Then delete leaf nodes in left tree and right tree.
- If after this the root becomes a leaf node we do that check again.
- then return root as per pointer reinforcement.
- Post order check guarantees that we address situation where children get deleted and the node becomes a leaf.
- This also works on a leaf node because its left and right children are null so we delete it, and this pattern follows up.

### Maximum Path:
- DFS will find the max in only one way (left or right).
- From here, for every node, we max out a global variable seeing whether the particular node can be the middle and serve as the midpoint of the path.

### Serialize and Deserialize Tree:

### Straight forward:
- In serialize, call a dfs that separates nodes by commas and then pushes a “N” character to stringstream.
- Then do preorder processing (insert current node)
- recurse left and right.
- convert stringstream to a string with .str operation.

### Then to get the string back:
- use i global counter.
- if current value is “N” return nullptr.
- if its a “,” move to the next
- if we are at the end of the string return nullptr
- then see whether we are at a “-” indicating a negative number.
- if so set a boolean flag.
- while we don’t hit a comma, multiply current value by ten and add value of string - “0” (ascii), and then increment i pointer.
- create a new tree node on the heap, if it was negative set it to negative value else set it to positive value.
- then increment i and recurse left
- then increment i and recurse right
- return root.

### Trie:
- Create a node mapping character to nodes
- Also if word end.
- Initialize a new one
- Then for adding word, create a traversing pointer, and then go through each character in word (size_t), and then if its not in the map create a new node that maps character to that node.
- Then at the end set word end to true.
- For search same thing, just if it doesn’t have the word, just return false. At the end check if word end true is set.
- For is prefix, just do the same as search, but no need to set word true since we care about prefix.

### Design and Add + Insert Words:
- Wow a lot easier this time.
- We do standard add.
- But wildcards cause a DFS where we consider each entry at the current node level and see if any of them return true.
- DFS: takes trienode, current character index and the word (string view because more permissive)
- if the current index is longer than the word, then we return whether the current node is a word end.
- else, we init pointer node to head go through a for loop and iterate through the word.
- if the for loop variable is a ‘.’, go through all next nodes for the current pointer, see if a dfs returns true, and then return that.
- else if the node doesn’t have the current character return false.
- else we move the pointer to the next node.
- at the end of the for loop, we return if this is a word end or not.

# Extra Characters in a String
- This is DP
- At every character you have a choice of skipping it and adding 1 to the extra characters.
	- However, if this character is the start of a word in the dictionary, we minimize the number of extra characters in this string with running dfs on everything starting from the last character in the string.
	- go through all dictionary words, get a string_view of the current string with the bounds of the current position and the word length.
	- check for equality.
- Repeated work: so we can memo it.
- Can obviously do bottom up if necessary. 
- can use a trie, but i think that's overneeded here because this is a $O(N^2*d)$ solution.
	- But if trie is required, build a trie out of all dictionary words, what we do is iterate in the substring and for each character, go through the trie and see if its a word end and then do the min.

# Word Search II
- We do depth first search alongside trie nodes.
- Create a trie node, and then a Trie data structure with a root initialized.
	- Trie node stores the word itself as well.
- Create a visited set that's a nested list of ```std::vector<std::vector<bool>>```
	- Done with ```std::vector<std::vector<bool>> visiting set(matrix.size(), std::vector<bool>(matrix[0].size(), false))```
- Add all dictionary items to the trie.
- Then, iterate through the array, and then if the trie's first layer contains any of the characters in the word, then do a dfs starting from the row, column, the root, and pass in board, visited, output.
	- In the DFS, check for out of bounds, see if visited, and also if the trie node doesn't have the current mapping of board cell.
	- If the mapping does have the current mapping of a word, and the word is an end, then add the word to the output, and then get rid of the endword flag.
		- This prevents duplicates from occurring when we iterate from another word.
	- Then backtrack: set this visited to true, then go through all four directions, and then set back to false.

# Sum of All Subset XOR Totals
- Straightforward backtracking.
- At each decision, keep track of the path XoR
- if the pointer exceeds the size of nums, return.
- Picking item means xoring the current item with the path xor.
	- then add that to the total.
- dfs to the next index with this change in the path xor.
- unpick is just going to the next index with the existing path xor.
- XOR properties: a ^ a = 0 (XOR'ing with itself is 0), a ^ 0 = a.
	- There is a more complicated bit manipulation version, look into that when there's an interview.
		- Essentially, when XORing, an active bit in a number will be present in half of the subsets. (2^n / 2 subsets)
		- So we need to find all active bits, and then we can multiply that.
# Subsets
* Pick/unpick pattern.
* DFS takes a pointer, references to the nums vector, path vector, and a nested output vector.
* If the pointer exceeds size of the nums, then we push it back to the path.
	* the reason why this is good even through there's a copy involved is because we need a snapshot of the path so far (since this path is shared throughout the call stack because its a reference).
	* `push_back`: creates a copy, `emplace_back` avoids the creation of a temporary r-value object.
* Push item back and then go to the next element in nums, and then pop, and then go to the next element.

# Combination Sum
* Again, pick/unpick pattern.
* DFS takes pointer, reference to nums vector, path vector, output vector, and the target sum.
* If this sum is a 0, then we add the path vector to the output vector.
* If the i vector exceeds nums size or equals it or if the target sum is less than 0, prune (return).
* if we pick, we can stay at the same place and reuse, but our target sum just decreases by the nums.
* but if we skip, we forever move forward.
* this is why we have unique combinations: we never allow for going backwards (only go forward).
* In terms of time complexity, its $(O(T/min(N) * 2^n))$, copying with the largest possible size times there's two choices.
	* However, a more honest bound is $2^(N+ T/m)$
		* $T/m$ = number of times you can take at max
		* N = numer of times you can skip. 
* Can optimize with early stopping, sort and we know that if our candidate is higher than the target, then we return.

# Combination Sum II
- Pick/unpick
- To avoid duplicate cases: sort the input, and when unpicking, don't pick any of the same elements as this. 
	- Go to the next unique element.
- In both cases we do `i + 1` because we want to get to the next unique element.
- Don't try to skip duplicates at the top level because this would force all unique elements in each of the containers.

# Combinations
* Need to avoid duplicates.
* We maintain i (current number), the output vector, the path vector, n (max number), and k (max elements in our list).
* If the size of path vector (remember to `static_cast` to int) is equal to k, we can push it back to output.
* If i is greater than max number we can be, or if the size of our path is greater than the max (k), return.
* We push this element to path, and then we go to the next element, else we pop and do the same.
# Permutations
- Idea: all permutations are generated by swapping with everything that's greater than the number.
- So we iterate in an inner loop, and swap with every index greater than equal to, and then do dfs(i + 1).
	- We do greater than equal to consider the state where we don't swap the first element.
- To backtrack and consider another swap, we revert the swap we just did.
	- Just do the swap operation again.

# Subsets II
- Idea: pick/unpick
- To avoid duplicates, when we skip items in a particular layer, we move past all elements and recurse on the next unique element after sorting.
	- So keep moving while `nums[i] == nums[i - 1]`.
	- Then the next index for i is an unique element, backtrack starting there.

# Permutations II:
	Look more into logic of this tomorrow

# Generate Parenthesis:
- Keep track of open, closed, and n pairs.
- If open == close == n, we are done, so we push the path to the output.
- if our closed ever exceeds open, return (illegal).
- if our open is more than closed:
	- we can add either an open bracket or close it.
	- have closed logic, push "(" into the path.
	- Then recurse that way.
	- Pop
- If closed equal to open:
	- forced into a open bracket.
	- push ")" into the path.
	- recurse that way
	- pop.

# Word Search:
- Do DFS on the grid with the word in mind.
- If our word pointer is at the size, word has been successfully matched, return true.
- if our r, c indices are out of bounds, the current square doesn't match the word index in the word, or the current location is visited, return false.
- set the visited to be true for this path
- then recurse in all directions and or it to see if from this square we can get the word.
- if we permanently mark a node as visitied, we can't form words that may include that letter from other paths.
	- so we mark it as not visited after we have finished procesing all other paths starting from that square.

# Palindromic Partitioning:
- For each index, consider all substrings and see if they are palindromes through auxillary function.
	- if it is, place it in a vector of our current path of strings.
	- then recurse to j + 1 (because we processed i to j inclusive), which is why the `std::string_view` constructor goes from `s.begin()` to `s.begin() + j + 1`. 
	- to explore other possible palindromes and reset our state, we pop this from the current path.
- Remember to use a std::string_view for partition because its quicker.
	- `emplace_back` if the string view is a palindrome.
- Optimization:
	- Precompute all partitions that are palindromes (DP table of two dimensions containing information about whether its a palindrome for two indices).
	- then given two indices, can just do a `O(1)` lookup.

# Letter Combinations of a Phone Number
- For every single number, you have three choices.
- So create an `unordered_map` that maps every number as a character to the string of letters that correspond to that character.
- Then in the dfs, standard procedure for adding (if i pointer equals the size of digits, append the string made to the output vector).
- for every single number, iterate over characters in the corresponding string mapped in the dictionary.
	- and then recurse to the next index.
- after we reach back to this point, undo our work/restore state by popping the letter that we picked.
	- then go to the next character.


# Matchsticks to Square:
* Question devolves to having four partitions of an array that have equal sums.
* So have a vector of four groupSums and add each to the target.
* then recurse to the next position (since requried every single matchstick used).
* THis TLES, so optimizations:
	* We know each group should be equal to the sum divided by 4 for a successful square since every stick is used.
	* So when checking if true, check if all groups are equal to this target.
	* And when adding to a group, check if if doing so exceeds this sideTarget (if so, we skip this).
* also sort max to least so that we fail fast.
	* do so w/ `std::sort(matchsticks.rbegin(), matchsticks.rend()`).

# Partition to K Equal Sum Subsets
- Similar approach to matchsticks to squares.
- Except, instead of a fixed size of 4 groups, its k.
- Calculate target sum of each group, and sort largest to smallest like usual.
- When calculating if something is a hit, check equality with the target.
- then iterate over all the sums.
	- if adding the current number would exceed the sum, continue.
	- since these bins are all interchangable and each number can only be in one bin, we have duplicate state spaces when we add to duplicate bins.
		- If we see the current group sum is equal to the previous one, we move to the next one.
	- We then add the number to the group, then recurse to the next index.
	- Then we revert state.
	- If the bucket is 0, that means everything to the right is 0 since we add to the left most bucket.
		- So if the bucket was initially 0, adding this element to everything to the right yields a duplicate, so we break from the loop.

# N Queens
- Classic backtracking.
- Keep track of board state as a `std::vector<std::vector<char>>`.
- Keep track of rows, cols, positive diagonals, negative diagonals w/ `std::unordered_set<int>()`.
- if we used up all queens (indicated by i), then we create a string off of the `std::vector<char>` constructor and add it to the result.
- Then iterate through all columns till n.
	- If the row and column are in the respective hashsets, continue to the next.
- Else, add appropriate values to correct hashmap.
- set correct row and column to `Q`, recurse to the next row.
- Revert all word

# N Queens II
- Same thing as N Queens but no need to store state.
- have hashsets.
- if i == n, return 1.
- then for each state, maintain a total count variable.
	- add to that if we are in a valid position.
- at the end, return this.

# Word Break II
- Keep track of all strings that map to the full string in a `std::vector`.
- Iterate through the word, see if the substring matches the main string.
	- If so push this string back onto the vector.
	- Then dfs to the next index (i + w.size()) that wasn't processed.
	- Then pop back.
- When adding a word to the result.
	- create a `std::ostringstream` and then add strings to this output and add spaces when it doesn't equal the last element.
	- Then, construct a string from this, and push it back to the result.
	- Then return.

# Kth Largest Element in a Stream
- To create a min heap: `std::priority_queue<int, std::vector<int>, std::greater<int>>`.
	- Max heap: `std::priority_queue<int>`
- U can initialize this through constructor list, and while the size of the min heater than k, pop.
	- So we only have large elements.
- Then, when we add, u push to the heap, if its greater than k, we pop.
- Then we return the top.

# Last Stone Weight
- Create a max heap of the stones.
- Then while we still have weights in the max heap:
	- pop largest and second largest, then if the largest is greater than the second largest, then we push to the max heap the difference.
- at the end, if our weights heap is empty, we return a 0.
- else we return the top of the heap.

# K Closest Points to Origin
- Don't use build heap here because this means that heap operations will be of max size N not k.
- Essentially create a priority queue of pairs of distances (not square rooted because squaring is more efficient and we care about relative ordering), push to this priority queue.
	- If the size of the priority queue ever exceeds k then we pop.
- Then we construct an output by repeatedly popping from the priority queue and `emplace_back` the indexed point from the vector.
	- We need to first access w/ `top` and then we `pop`.
	- `top` returns a const reference so we can use it directly in expressions.
- There's a quick select solution, but i think its overkill (quick select makes sense for like kth largest).

# Kth Largest Element
- Easy approach: use a min heap and pop when we exceed k
	- This leaves us with the k largest elements.
	- The top of this heap will be the element of choice.
- Harder approach is quick select:
	- Use `std::random_choice` to initialize random device
	- use this to initialize a `std::mt19937 gen{rd()}`
	- then within quick select, take a l and r (not inclusive) bound.
		- then init. uniform int distribution (l, r - 1).
		- get the pivot index
		- then get the value
		- from there, swap this with the last most element.
		- then iniitalize a i pointer and left placement pointer w/ l.
		- iterate from i to r, if the `nums[i]` is less than the pivot value, swap with the left value (place it there), and increment left pointer.
		- increment i.
		- from there, left represents the first number greater than the pivot so we should place it there.
		- kth largest is located at the index n - k.
			- if this is equal to index of left, return that number.
		- if the left is greater than that, we found a number furhter to the right, so we recurse left (l, left).
		- else, we recurse left + 1, r
			- we do + 1 because we already know the left element isn't the kth largest.
		- USE LEFT NOT PIVOT.
			- PIVOT index doesn't represent the location of the pivot after its been partitioned.
			- 