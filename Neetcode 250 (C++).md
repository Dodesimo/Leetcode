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

# Task Scheduler
- Super unintuitive for me.
- Idea: track of frequencies first.
- Then have two priority queues:
	- job queue: max heap storing # of tasks and the task character `std::priority_queue<int, char>`
		- To minimize time taken, since we have a `n` gap, most frequent items will require the most gaps (using them early allows less frequent tasks to fill gaps).
	- waiting queue: min heap that stores time till we can go back along side remaining frequency and the character task:
		- `std::priority_queue<std::pair<int, std::pair<int, char>>, std::vector<std:pair<int, std::pair<int, char>>, std::greater<std::pair<int, std::pair<in, char>>>>`
	- strict invariant:
		- job queue always has valid jobs running on it.
		- waiting queue has jobs that are waiting.
	- while waiting queue is not empty or the job queue is not empty (so loop stops when both are):
		- if the waiting queue has things and the time something can come back is equal to the global time (set to 0):
			- get that item and put it on the job queue.
		- if job queue has things in it:
			- get the top (don't have to do any checks because we know that its not in the waiting queue).
			- decrement the instances
			- and then add to the waiting queue with a time that is t + n + 1 and then new copies and the job
				- reason is because n represents intervals.
				- at least n intervals, we can run on the n + 1'th interval.

# Design Twitter
- Keep track of people user is following: `std::unordered_map<int, std::unordered_set<int>>`
	- maps user id to a vector of following IDs
- Keep track of users tweets:
	- `std::unordered_map<int, std::vector<std::pair<int, int>>`
-  keep track of time
- every post: push back the time and the tweet id for the right user, then increment the time
	- this produces a sorted list of tweets for each user.
- getNewsFeed: 
	- since greater time means more recent and smaller time means less recent, use a max heap to store tweets.
	- since the user needs to be in the following, insert userId into following set for user.
	- then go through all following, and then if the following person has tweets (greater than 0), get the most recent tweet for that person at `tweets[f][tweets[f].size() - 1]`
	- then push to the max heap a vector of the time, tweetId, nextIndex (which is the `tweets[f].size() - 2` (cast to int), and the userId of the follower F).
	- then create a results vector, and while its size is less than 10 and the maxHeap size is not empty get the top item in the heap.
	- push this back onto the results vector, and if the nextIndex is valid (so greater than equal to 0), get the next tweet through this index, and then push to the max heap the details of this tweet, the next index, and the userId of the tweet.
	- then erase userId from following
	- return the results vector
- follow: just add it to the set
- unfollow: if the follower is in the keys of the followerId and the foloweeId is in the hash set that the followerId hash, then erase.

# Single-Threaded CPU
- We need two priority queues.
- One priority queues has all the tasks in terms of their arrival times, processing times, and the index.
	- create a vector with this information, and then use priority queue iterator constructor to heapify this
		- `std::priority_queue<std::vector<int>, std::vector<std::vector<int>>, std::greater<std::vector<int>>> queue (tasksWithIndex.begin(), tasksWithIndex.end()`
- then have a long variable for time
- and create separate priority queue: `std::priority_queue<std::vector<int>, std::vector<std::vector<int>>, std::greater<std::vector<int>>> readyJobs`
- while we have jobs in either, while the size of our queue is greater than and the top of the queue is less than the current time, we push the processing time and index into the priority queue, and then we pop it.
		- remember `top()` doesn't remove the item from the queue
			- so you have to pop
- If there's a ready job available, get the top, and then pop it, and from the top, put the index in the result vector
	- we handle the question's requirements through `std::priority_queue`'s lexicographic heap ordering
- then we fast forward time to the processing time + t.
- however, if there's not anything in the ready jobs, can fast forward time to the least time in the first priority
- don't try to manually increment time, fast forward to the next time.

# Reorganize String
- Create a frequency map that maps ASCII offsets to frequency.
- Create a frequency max heap (`std::priority_queue<int, int> frequencyMaxHeap {}`)
- Then for each frequency, emplace it in the priority queue.
- Then maintain a pair in the previous that maintains the frequency and ascii offset (this can be set to -1, -1)
		- Could also make it 0 in order to simplify the logic where the previous is set to the correct value after the frequency of the character is dropped
- If the maxHeap is empty and we have the default, return an empty string
- Then get the top character on the frequency, pop it, get the actual character by adding `a`, then decrement the frequency.
	- If the previous is empty, place the previous in the frequencyMaxHeap again
- then if we still have characters, set it to previous, else set it to default.

# Longest Happy String
- Create a max heap of the character frequencies in a `std::pair` (remember first item in pair is int, second is char, counter + character)
- while the max heap is populated, get the top, pop it
	- if this has greater than 1 frequency, if we had a previous and the previous has more, we are careful and only use one character
		- decrement counter
	- else we use two (because we can't form three)
		- decrement counter
	- if the frequency is only one, decrease and add the character to a `std::ostringstream`
- have the previous be initialized to 0, and if the previous isn't 0, go ahead and add it to the available stream again
	- then, set previous to the `std::pair` that we popped
- then at the end return output from a string
- tips:
	- don't set pair to -1 and then do previous checks it makes it complicated. just set the count default to 0, check if previous not 0 then add and then update previous
- better solution: 
	- always add the highest frequency character to the string
	- if doing so would mean the last three characters are equal, use the second highest
	-  else add highest frequency, don't add if frequency becomes 0

# Car Pooling:
- Interval question w/ two approaches:
	- heap:
		- sort the trips array in terms of starting time
		- then we maintain a priority queue of pair containing the last index and the # of passengers at that point
		- maintain total variable
		- add the first trip passengers to total
		- also add this to the heap
		- go through the rest of the trips, and while the priority queue has items and the end time (heap top) is less than or equal the current trip's start time, decrement their passenger count from the total and pop
		- then add to total, check if exceeds capacity, if so return false
		- then add to the heap the end time and the passengers
	- line sweep:
		- idea for every time interval go through and add or subtract the number of passengers to simulate the net
			- make a vector the size of the total amount of intervals
		- then go through each time step (index of the interval array), and add up totals (if it exceeds capacity, return false, else return true)

# Find Median From Data Stream
- idea: left heap gives you the max of the left side of the data, right heap gives you the min of the right side of the data
- when we add values, the logic is:
	- if this value is less than the left side's max, we know it should be there so insert
		- otherwise, add to the right side
	- if the difference between the two heaps exceeds one, then move from larger heap to smaller heap
	- then when calculating the median, if both heaps have the same size, average of the two
	- else whatever heap w/ more items has the median (beacuse odd number means one median, should be either to the left of the larger numbers if less or to the right if the smaller numbers are less)

# IPO
- this is a scheduling problem (realized too late)
- projects get available when w becomes >= than the capital of the project
- all scheduling problems have a waiting queue and an actual queue
	- here, create priority queue of just project profits (max)
	- create another priority queue of schedule times + profits (min heap based on first item)
- while both heaps have items, move over the profits of projects to the max heap when we have capital >= their required capital
- if we don't have any possible projects in our profit, we break
- then we pop the greatest profit (since this is a possible item) and then we add it to our capital increment our counter, if its == to the limit, then we break
- at the end we return our capital
	- this capital is init. to w


Rest questions I will do in Python because I have enough experience w/ C++ coding now

# Island Perimeter:
- a land's sides is determined by number of 0s or if boundary

# Verifying Alien Dictionary
- map each character to dictionary
- iterate through the first and second words in a zip, set an index counter
- then set a not match flag  indicating whether between the two words there's a match
- iterate through two words, if both characters equal, move on
	- if characters differ: if first character is less than second character, good, set notMatch flag and break
	- if first character > second character: return False (violates the ordering)
- after comparing these two words:
	- if prefixes equal and smaller word is the second one: incorrect ordering, smaller prefix should go first
- return True at the very end

# Find the Town Judge
- initial approach: keep an adjacency list of trusted, add everyone to this list first and then process trusts
	- then iterate and find someone that's not in the keys but in the values
- maintain a difference array of size n + 1:
	- town judge has an indegree of n - 1 and an outdegree of 0
	- for a, b where a trusts b, we mean indegree - outdegree (that needs to be n - 1)
		- so a's value gets decremented, b gets incremented
		- then we see from range 1 to n + 1 there's a value that's n - 1

# Number of Islands
- this has been done a million times

# Max Area of Island
- just number of islands, but instead of returning void, if the thing is out of bounds or is an actual 0, return 0
- else, add to visited set, set a total to 1, and then increment that by `dfs(r + 1, c), dfs(r - 1, c), dfs(r, c - 1), dfs(r + 1, c)`
	- return total
- keep track of max across all dfs starting at a valid one state and then return the value

# Clone Graph 
- Map the old nodes to the new nodes
- do a dfs on the old node
	- if the old node is in the hashmap, return the old node's mapping
	- else:
		- create the new node
		- set the mapping in the hashmap
		- then set the new node's value
		- then set the neighbors through a DFS appending to neighbors list
		- return the new node

# Walls and Gates:
- idea: we do a BFS from the gates to the walls in order to determine closest walls 
- when enqueueing nodes into the queue, add to visited
	- this prevents duplicate nodes in the list which can pose efficiency/correctness concerns
- think one ahead (don't need to, can just change value to layer within the main loop)
	- add function should add to visited set, enqueue, and also set value
	- queue used to set all nodes nearby one
- to iterate over a layer, calculate the length of the entire queue and then for loop over that

# Rotting Oranges
- idea: 
	- keep track of fresh oranges and rotten oranges, rotten oranges get put in queue
	- adding: check for bounds, then add to visited set, then set it to a 2 and then add it to the queue
- main loop:
	- while we have stuff in queue AND we have fresh > 0, get the length of the queue, iterate over it, and do add function
	- increment time (which serves as our layer)
- if we break out of this main loop we know either our queue is empty or our fresh is 0
	- if our fresh is 0, we found a possible time
	- else -1

# Pacific Atlantic Water Flow:
- instead of running a dfs from every cell and seeing if both shores are reached, reverse thinking?
	- from ocean can i reach this cell
- for that, run dfs from all perimeter (row 0 means pacific, col 0 means pacific, last col means atlantic and last row means atlantic)
	- each dfs returns if previous value is greater than current
		- this is because that means the water couldn't flow since heading from cell to water, everything needs to be smaller (5 4 3 2 1)
		- starting from the water to the top, each previous needs to be smaller than last
- each border type DFS has own visited set
- so we just find the union of these visited sets, because they represent cells that can be reached from each particular ocean
	- union: both oceans 

# Surrounded Regions
- think about the reverse
- all 0s in the border can't be surrounded because of the wall
- do a dfs on them and mark them w/ temp symbol
- iterate through the grid after and if its equal to 0, its truly surrounded so change to X
	- if its a A, change to O (this couldn't be captured)


# Open the Lock:
- shortest path in a unweighted graph: BFS
	- don't try DFS because that's just any path that gets you the target, so not efficient
- deadends are added to visited set, so is the original sequence (as strings) then added to the queue as a list
- adding logic:
	- get a string version of the result
	- check if in v if so return
	- then add to queue as a list (COPY since python passes everything by reference)
	- and add to visited as a string
- adding:
	- while queue has items iterate over each layer
	- pop the list from queue and then for each item in the list
		-  if we pop and its equal to target return that
		- increment one modulo 10
		- go back (reset) by decrementing one modulo 10
		- then decrement one modulo 10
		- go back reset by incrementing
		- add each variant
	- else:
		return -1
- og approach: use DFS but this checks everything
	- before running algorithm think about what makes it easier
- check whether the initial spot is a deadspot

# Course Schedule
- Create adjacency list b -> a
- create visiting and visited sets
	- visiting tracks the current path through the node
	- visited is nodes completely processed
- if we end up at the visiting set, return False (loop)
- if we end up at the visited set, return True (because we already explored this node completely, this doesn't cause a loop)
- add node to visiting set
- then for all neighbors, do dfs and see if valid
	- if not return false
- move node from visiting to visited
- return true
- then for every course run this algorithm
	- not just 0 because other disjoint piece could have a loop
- if any component yields a false return false
- else True

# Course Schedule II
- Same idea as Course Schedule 1, but while checking for loops, we construct a topological ordering of the graph
- so after we process all neighbors and move to visiting, we add node to our result
- then we run this algorithm on all courses:
	- not just 0 because there could be loop in other disconnected component preventing all courses from being done
	- also, because if there's no prereqs, we anticipate a random ordering of all independent courses
		- processing all neighbors handles this
- if any DFS returns a false as part of a loop, return empty list since no valid ordering
- else return the result

# Graph Valid Tree
- Key facts:
	- Tree is a graph that's fully connected (one fully connected component) and has no cycles
	- tree has exactly n - 1 edges (more not a tree)
- create an adjacency list that bidirectional
- have a visited set
- have dfs function that accepts a node and its previous (to avoid case where loop exists from going back to parent)
	- if node in visited return false
	- then add to visited
	- then go through all neighbors, if its equal to previous continue
	- then do dfs on neighbor, if returns false return false
- then at the end return true
- for one connected component, need to run from the origin (0) and see whether the visited set is exactly the size of n
	- reuslt is the logical and of dfs and the size of the visited set

# Course Schedule IV
- trick: precompute prereqs
- for each node, track visited nodes and store them in a 2d grid w/ Trues showing they can be reached from the node False if not
- then for each pre req, course, return the result from the 2d grid
	- this works because going from A to B implies A is a prereq to B

# Number of Connected Components in Undirected Graph
- create adj. list of undirected graph
- then do dfs w/ previous and just skip when previous = neighbor (our parent) (return when node in the visited)
- then run this dfs on every single node that is not in the visited and increment count

# Redundant Connection
- Union Find is easiest
	- approach:
		- ranks list with list 
		- parents with each node as their own parent
		- find:
			- set current to node
			- while the parent of current isn't equal to current (traversing up the tree)
				- collapse: `self.parent[current] = self.parent[self.parent[current]]`
			- return current when condition not true
		- union:
			- get the parent of both nodes
			- if they the same return False, couldn't union
			- else diff and if rank of one is greater than the other, greater one's rank gets added to and greater one becomes parent of the other
- then iterate through edges in sequential order and then do union find and if something fails return false. 

# Accounts Merge:
- index maps to unique person
- maintain a map of address to index
	- if the address already exists, merge indices
- then, create a parent to email mapping
	- find the parent for each index for each email
	- then append the email to that parent
- iterate over all parent, email lists
	- create a result list
	- for each parent, email
	- add to the result the parent and the sorted email list
- return result

# Evaluate Division
- Graph question
- if you know a / b = 5, you know to get from a to b you can multiply 5 by b
- so create an undirected two-way graph
- then we do a dfs where we have a result and takes parameters current, previous, path, goal
	- if we reach goal update result and then return true
	- if in visited set return false
	- add to visited set, then do dfs on all neighbors (if previous == since undirected continue), check if true if so return true
		- dfs on all neighnors takes the path and multiplies w/ multiplier 
	- at the end return false (means we couldn't reach the end from anywhere)
- then have answer 
	- if the start of query not in graph, append -1
	- then initialize path value to 1, and then run a dfs (with start, end, path, value, previous default to float)
		- if we reached, then append the path value
		- else append -1
	- then reset the visited set

# Find Min Height:
- min height exists at the midpoint
- so we essentially want to go inwards from leaves and progressively remove leaves in order to find the min height till we get 2 nodes
	- we can have one or two midpoints
- create undirected graph adjacency list
- create a deque for leaves and a dictionary for edge counts
- go through all nodes
	- if the length of the adjacency list is 1, add it to the deque
	- then set the dictionary value
- then while the deque has values,
	- if n is <= 2, return the deque as a list
	- get length, then pop node
		- decrement n
		- for each neighbor, subtract the edge count by 1
		- if this results in a value of 1
			- this is now a leave (only edge goes backward)
			- add this to the leaves deque
- at the end return deque as a list

# Word Ladder
 - this is a graph problem
	- need to go from start to end as the middle words as transitions
- to determine whether a middle word is possible to move to
	- the current word and that word must be separated by one character difference
	- this can be maintained by seeing if the two words have the same wildcard sequence where one character is represented as * and everything else remains the same
- create list of wildcard to to word mapping
	- add beginning to visited and queue
	- pop from queue
		- check if this word is the end if so return layer
	- for each word, calculate wild card
	- for each wild card to word mapping, add those words to queue and visited set
	- increment layer outside
- at the end of the queue, return 0 (means we couldn't find answer)

# Climbing Stairs:
- dfs:
	- if i == n, 1 way to get to the top so we return 1
	- if its greater than 0, 0 ways
	- from i, # of ways to get to the top is dfs(i + 1) + dfs(i + 2) (top two)
- memo:
	- we have a lot of repeated sub problems so cache them through a memo
- dp table:
	- n + 1 entries: each represent a state (starting from 0)
		- represents # of ways to get to state n from that position
		- last entry is 1 because only one way to go from n to n
	- then iterate backwards and if i + 1 and i + 2 are in bounds add them to dp entry
	- return `dp[0]`: first state

# Min Cost Climbing
- dfs:
	- if i >= len(costs), cost is 0 because we exceeded the target
	- else we pay current cost and the minimum of either going one step or two steps
- memo:
	- since there's repeated subproblems we can cache our results
- dp table:
	- same # of entries as the input array
	- each entry represents the best cost from beginning and including to that value
		- last value would just be the cost since we can always go to the end
	- for i + 1 and i + 2 see if bounds if so the values retain, else they are 0
	- then add the minimum of those two added to cost to the dp entry
	- then we find the minimum of the first two values
# Nth Tribonacci Number
- dfs:
	- if we reach the base cases just return those
	- else, return dfs(i - 1) + dfs(i - 2) + dfs(i - 3)
- memo:
	- a lot of repeated work so use a memo
- dp:
	- if we have base cases just return immediately
	- else create a dp of size (n + 1), since we go from 0 to n 
	- then just iterate from 3 to end of DP array and then return the last item
# House Robber:
- dfs:
	- if your i is greater than equal to the length of the array, not a valid index so return 0 (can't rob anything)
		- for a particular index, can choose to pick it and then skip one (`value[i] + dfs(i + 2)`, or skip to the next index (`dfs(i + 1)`)
- memo:
	- since there's repeated subproblems, you can cache in a dictionary
- dp:
	- each index in dp table represents best profit there
	- for the last index in the dp table (same size as the input array), its the `nums[-1]` value because its the last house (most you can do is rob it)
	- then for indices that are valid, apply the above recurrence relationship

# House Robber II:
- if the first is connected to the last, we get two options:
	- rob first, can't rob last (0 to n - 2)
	- don't rob first, rob last (1 to n - 1)
- dfs:
	- dfs takes an i and bounds:
		- same logic as above if i >= bounds return 0
		- otherwise  `max(dfs(i + 1, limit), nums[i + 1] + dfs(i + 2, limit))`
- memo:
	- repeated subproblems, cache
- DP:
	- two dp arrays: takeOne and skipOne
	- to do takeOne, we iterate from len(takeOne) - 2 since we skip last position, and then apply above recurrence relationship
		- the bound is < len(takeOne - 1)
	- for skipOne, we iterate from len(takeOne) - 1 all the way up to position 1 
		- the bound is < len(takeOne) (since its the entire array)
	- then at the end, we take the max of `takeOne[0]` and `takeOne[1]`

# Longest Palindromic Substring
- dp:
	- i guess the idea is that individual characters are palindromes, and then if i + 1 to j - 1 is a palindrome, i to j is a palindrome if `s[i] == s[j]`
	- this is dumb though
- best approach:
	- expand outwards while its palindromic, check if its greater than length and if so update global L and global R and the length
	- need to check even and odd palindromes (i, i and i, i + 1)

# Palindromic Substrings
- same idea as longest palindromic substring, but just expand outwards while palindromic and increment counter while adjusting l pointer and r pointer
- check even and odd palindromes

# Decode Ways
- brute force:
	- if we exceed string length or we start at a 0 return 0
	- if we are at end of string return 1
	- initialize total, add dfs of going to next position
	- then if we can formulate a valid number between 0 26 inclusive, dfs to i + 2 (skip next digit)
		- this means we need to check next index is valid
	- then return total
- memo:
	- we have a lot of sub problems so cache
- DP:
	- we have len(s) + 1 states (0 index to end of string)
	- last is always 1
	- then we iterate backwards and if we don't start with a 0
		- add `dp[i + 1]` if in bounds
		- if this position and next number formulate a valid number, add `dp[i + 2]`
		- only if bounds, else add 0
	- return `dp[0]`

# Coin Change:
- brute force
	- run dfs with amount, if amount < 0, return float('inf') because not valid 
	- if amount = 0, return 0 (no more coins needed to get there), total = float('inf'), then iterate through all coins, min total with 1 + dfs(amount - coin) (we used one coin and we have amount - coin left)
- memo:
	- same idea, subproblems so cache
- DP:
	- 0 through amount states, amount - 1 states
	- last entry is 0, go from amount to amount requires 0 coins
	- then iterate backward through dp table
		- iterate through all the coints
			- if value + coin <= amount:
				- `dp[i] = min(dp[i], 1 + dp[i + coin])`
				- the number of coins needed for the remainder is one coin + the number of coins needed to make the rest of the remainder
# Maximum Product Subarray
- this is like Kadane's algorithm
- keep track of a minimum, maximum, globalMinimum, globalMaximum
	- each of these are set to the first element to handle the edge case of just one element
- for each element from index 1 and onwards,
	- `newMinimum = min(nums[i], minimum * nums[i], maximum * nums[i])`
		- either start the minimum subarray from this element or extend it where minimum and maximum are the encountered minimum or maximum so far
			- we check both because of the situation a negative times negative = positive
	- do the same for the maximum just w/ maximum
	- then update the global minimum and maximum with these new values
	- and then set the minimum, maximum to these new values
- return the maximum
- need new values for minimum and maximum because we don't want to reuse intermediate values in the calculation of the other

# Word Break:
- recursive:
	- if i > len(s) return  False
	- if its exactly equal to len(s) return True
	- then iterate through all of the words, if current i + len(w) substring of s is equal to the word and dfs(i + len(w)) is good, mark variable as true and then break
	- return the variable
- memo:
	- repeated subproblems (breakage possible at i)
	- cache results
- dp:
	- table of size (len(s) + 1), 0 through len(s) states, 
	- last state is true, breaking nul; string into the S is always possible
	- then we iterate backwards through i, and for each word, if the length of the word + i is within bounds and the substring matches, we can break from this point
	- then set the `dp[i]` to the one thats `dp[i + len(w)]`
	- return `dp[0]`

# Longest Increasing Subsequence
- recursive:
	- every single number by itself is a LIS of 1
	- so the base case is that if we equal the the last element thats automatically a LIS of 1
	- then initalize LIS of 1
	- then for each element to the right, check if its greater and if so, max LIS with 1 + dfs(j) (extending the LIS of index j by one)
	- then return LIS
	- since LIS can be anywhere in the array, we do DFS on each position
- memo:
	- we do repeated subwork so cache results
- DP:
	- dp table initialized as 1 (every item is its own LIS)
	- create dp table w/ same size of input array
	- then iterate backwards through dp table
		- then iterate for each number from i to len(dp)
			- if its greater than number at i, update the dp entry with the max of itself and then `1 + dp[j]` since we extend the LIS of J
	- then find max across DP array

# Partition Equal Subset Sum
- recursive:
	- if the sum is odd, we can't do anything
	- get the sum, divide by two
	- then do pick unpick strategy and see if we can pick a subset 
		- `dfs(i, left)`
		- if left ever goes negative we return false
		- if i ever exceeds the length of array return false
		- if i == len(array) and then left == 0, return true
		- then we or `dfs(i + 1, left)` and `dfs(i + 1, left - nums[i])`: pick or unpick
- memo:
	- memo we have repeated subwork
- dp:
	- create a 2d table, fill out, look more when we do a 2d Question

# Combination Sum IV:
- recursion:
	- can't do pick/unpick because you need to access all the elements
	- do dfs with a target
	- if the target ever drops below 0 return 0 not a valid way
	- if the target is equal to 0, that's one way
	- for each number, increment a total w/ dfs(target - n)
	- return the total
- memo:
	- since each dfs(t) state represents the number of ways to get to target starting at that t, we can cache states
- dp:
	- dp table representing states from 0 to target (target + 1)
	- last item is 1 (1 way to get from target to target just don't do anything)
	- then iterate from dp backwards, then for each n, if i + n is in bounds
		- `dp[i] += dp[i + n]`
		- if i can get to i + n, the number of ways i can get to the end can be incremented by the number of ways i can get to the end from `i + n`
		- do this for all n to get all possible ways
	- return `dp[0]`: represents number of ways to get to the target starting with an initial sum of 0
# Perfect Square:
- recursion:
	- have a "left" variable for dfs
	- if its negative return float('inf') not a valid value
	- then have a min moves
	- then calculate perfect squares:
		- go backward because that's more efficient since larger perfect squares are more likely to cause in least sums needed
		- your limited by the left, so iterate from 1 to sqrt of left
			- and then minimize the min moves value w itself and 1 + dfs(left - the calculated perfect square)
	- return the the min value
- memo:
	- we have repeated subwork across states, so we put this in a dictionary
- dp:
	- create DP table from 0 to target (target - 1) entries
	- each entry represents the number of perfect square sums needed to make that amount
	- so for the last entry, its 0 since we are already at target
	- then iterate backwards w/ loop variable i, and then calculate from 0 to sqrt(n) (those are potential values of sqrt)
		- then if i + (s times s) is less than the length of Dp table
			- `dp[i] = min(dp[i], 1 + dp[i + s*s]`
				- to get to `i + s*s` we used another perfect square so its 1 + that
				- looping across all potential reasonable perfect squares (and s * s <= n - 1, since n - i is what's left)
# Integer Break:
- recursion:
	- calculate what's left
	- if left < 0: return float('-inf')
	- then calculate for range 1 to left (left is exclusive because we can't do a split to 0)
		- maxSplit = `max(maxSplit, i * dfs(maxSplit - i), i * (maxSplit - i))`
			- second entry: split the rest 
			- third entry, don't split the rest, keep it
	- return the max
- memo:
	- we have repeated subwork for the maximization product of integers for what's left
- dp:
	- create dp table from 0 to n
	- set first to 1: breaking 0 into max product just one thing
	- iterate from 1 to len(dp) table:
		- represents the total
	- for each total, we have a start, and then we apply the above recurrence:
		- `dp[total] = max(dp[total], start * dp[total - start], start * (total - start)`
			- second: we split the remainder
			- third: we keep the remainder
	- last entry will represent the max product from splitting n
	- return `dp[-1]`

# Stone Game III
- recursion: 
	- idea: instead of modeling each person's score, model score advantage one person has versus the other
	- if end of a array return 0
	- initialize a max, then maintain a sum
	- iterate from i to min(i + 3, array length)
		- add value to the number
		- max the max value w itself and the sum chosen - dfs(j + 1) (since that's the other max advantage)
	- return the max
	- if dp[0] positive return Alice else if negative return Bob else return Tie 
- memo:
	- since we are doing repeated subword cache
- dp:
	- dp table: array of (length + 1)
	- last entry: 0
	- iterate backward
	- have a sum variable and then iterate from (i, min(i + 3, array length))
		- maximize dp array with itself and the `sum - dp[j + 1]`
	- then do the same process as the start
	- each dp entry represents the max difference in score for the person at that position
		- 0 thus represents alice

# Insert Interval:
- at each interval, if the start of the interval is completely after the new interval, we can insert the new interval and then return the result + the rest of the intervlas
- if the start of the new interval is completely after the end of the new interval, the existing interval is added
- else there's overlap so min firsts and max lasts
- at the end we found the right spot to insert (everything before is merged and is the last position to insert)

# Merge Intervals:
- sort intervals based on start: `key = lambda x: x[0]`
- then add first interval to result
- and then iterate from 1 to end 
	- get the lastStart and lastEnd from the last item in results
	- if the current interval start is less than the last end, merge through `min(lastStart, currentStart), max(currentEnd, lastEnd)`
- return results


# Non-Overlapping Intervals
- idea: if we sort intervals by start, and process them incrementally as time proceeds, if we have an overlap, to minimize the # of overlapping intervals, we should maintain the interval with the smallest end to fit more intervals in
- doing this, sort the intervals, then maintain a last end
- iterate from 1 to end, and if there's overlap, make the last end the smallest end and increment
- else, make the last end the current end interval (moving through time)

# Meeting Rooms I
- iterate through, see if first of second is less than last of first (indicate overlap)
	- if so, return False
- else return True

# Meeting Rooms II
- two approaches:
	- line sweep:
		- go through all starts and ends, increment starts, decrement ends
		- then sort the hash map keys
		- then go through these keys, every start increments the number of rooms, max with minimum number of rooms required,  else decrement
	- two pointers 
		- sort start times and end times
		- if the start time is less than end time, increment start and number of rooms, max with minimum # of rooms required
		- else increment end time
		- move appropriate pointers for each situation
- # Meeting Rooms III
	- three conditions we need to maintain
		- have meetings be in the lowest number
		- no available rooms, delay until earliest room becomes free
		- room unused, earlier starts need to be handled
	- two min heaps:
		- one heap handles available rooms
		- one heap handles used rooms w/ end time and the room
	- iterate through the rooms in chronological order
		- pop things from used if less than equal to start time
		- add those rooms to the available rooms
	- if available is still empty, this means that there's no meeting available so we add it to available in the smallest possible room at endTime + (start - end) for the interval
	- then we pop from available (the room)
	- increase the count of the room
	- then, add to used the end time and the room number

# Minimum Interval to Include Each Query:
- naive:
	- iterate through all intervals for each query
	- and just do what it asked
- optimized:
	- sort queries and intervals
		- for queries, maintain the original index
		- create a result of the size of queries
		- interval pointer
	- we only pass intervals at once:
		- iterate through queries, and have interval pointer
		- insert intervals to a heap with interval size, start, end while the start is less than equal to the query
		- then push out everything that has a end <= query (that interval doesn't apply anymore)
		- from here, we have the smallest at the top and then add it to the original index in result
	- then return the result
# Lemonade Change
- greedy idea: if you need to make change for a 15, always prefer using a 10 and a 5 over three fives because fives are more useful
- also only maintain track of tens and fives because they are the only values that make sense to be used to make change
- iterate over the bills, if its a 5, we just add our number of fives
- if its a 10, we add to our tens and decrease our 5s (since we gave 5 in change)
- if its 15, if we have both tens and fives, we prefer a 10 and a five (subtract a ten and a five)
- else if the number of fives we have is greater than 3, we use those three (subtract three fives)
- else return false
- if in our transaction process we ever go negative, return False
- outside the loop return True

# Maximum Subarray:
- Kadane's algorithm:
	- keep a global and a running sum
	- the global is initialized to the first element (in an edge case like one element array, its just that element)
	- then iterate through numbers, add each number
	- update the global
	- if the run ever goes negative, make it 0
	- handles edge case of just a single negative number (since the subarray must be populated)

# Maximum Sum Circular Subarray
- Best normal non-wrapping subarray: globalMax
- best wrapping: totalSum - globalMin
	- the min could be continuous within, and if you subtract this
	- and the max wraps around
- so its either the Kadane's global max or total sum - Kadane's global min
	- the normal or wrapped
- calculate kadane's global max:
	- add each number, update global, if negative 0
- calculate global min:
	- add each num, update global, if positive 0
- if the sum is equal to the global min, all negative
	- so just the global max would be the maximum sum
- else return the max of Kadane's global max or total sum - Kadane's global min

# Jump Game:
- can do DP, but it goes to $O(N^2)$ because for each value in the array we iterate that much and see if the end is possible
- better: set a goal
	- this is the end of the array
	- iterate backwards through the array
	- see if the max possible thing reaches the goal or more.
		- If so we can get there
	- then move the goal to the current i position
		- because we know we can get from i to the end (inductively)
- then we check whether goal is 0 because we know sequentially we can only go backward if we could reach the end.

# Longest Turbulent Subarray
- we maintain track of the current run of alternating, result, and relationship of last state (-1 if numbers are equal or initial state, 0 if decreasing, and 1 if increasing)
- iterate through n - 1, and if current number is less than the next number
	- if we have seen the opposite  (relationship is 1), increment count, else make count 1 (just one element), then set the relationship to 0
- if the current element is greater than the next number:
	- if we have seen the opposite (relationship 0), increment count, else make count 1 (just one element), set relationship to 1
- both these situations are set to a count of 1 because these two relationships can be the start of a longer turbulent sequence
- if two numbers are equal, no way anything can be started so its 0 and relationship is -1
- the number of elements in a sequence of N alternating signs will be n + 1
# Jump Game II:
- Same idea with DP where we just track minimum values to get to the end from each position.
- Better idea, have a left and right pointer that maintains the range of reachable indices.
- while the right pointer is within len(nums) - 1, iterate over left through right, and calculate the farthest we can go from this range
	- this ensures we capture the "largest window" for each possible jump
 - then move left pointer to right + 1
- and then the right to the farthest
- then increment jump
- and at the end return jump
- this works because we iterate over each possible maximum-length window of jumps till we reach the end
	- the number of windows we go through correspond to the number of jumps we need to take
	- since each window is as large as possible, we ensure minimum jumps

# Jump Game VII:
- pattern of having BFS on array to do greedy search.
- DP works but TLES.
- Idea, have a queue that maintains indices and the farthest index seen so far.
- Pop from the queue and then add all indices from start jump to end jump that are 0s
	- we also keep track of the farthest index to the right because we don't want to repeat.
	- when we pop check if its the last index of string, if so return true. 
- start gets maxxed out with the farthest + 1 and the min jump plus current position
- iterate from this start to the minimum of the string length and index + maxJump (+ 1 to make it inclusive)
	- if the position is 0 add it to the queue
- then set the farthest to current + maxJum
- at the end, return False

# Gas Station
- Key observations:
	- if there are too much cost versus gas, return false (regardless of where u start, can never reach back)
	- if you start at gas station a and at index c you become negative, there's no b in between a and c because those would be better since c is the first negative prefix sum, and everything before is a positive prefix sum
		- starting after b reduces the amount of total net gas that you have that would be helpful so it wouldn't work
- thus, check if sum(costs) > sum(gas).
	- if so return -1
- maintain a sum and answer
	- add to the sum
	- if sum ever goes negative, set it to 0, answer becomes i + 1 (the current index)
- at the end return index
	- if we never go negative this would just return 0 (a lot of surplus)
- it's like Kadane's on the different array
	- except when we are negative, answer is the next index of where we are because we know anything before wouldn't help since we would cut out a positive prefix sum

# Hand of Straights
- greedy intuition: when making groups we always want to take the smallest possible card because that maximizes the number of cards we can have to force a in-order sequence within group size
- maintain a frequency of the cards
- then add all unique keys to a heap
- while we have the heap:
	- pop the smallest possible card
	- if this frequency is 0, then continue to the next one
	- then iterate from 1 to the groupSize,
		- calculate the expected card by adding to the smallest card
		- and then seeing if the frequency is equal to 0 (if it is, we know we can't make it so return False)
		- else decrement the frequency
	- after the loop, we decrement the frequency of the smallest possible value we just popped
	- if there's still items of the smallest left, we add to our heap.
- so the heap tells us the smallest available value to start our group with.

# Dota 2 Senate
- can't just rely on total number of each senator and then if ties compare to the first index
- the optimal strategy here is that each senator should eliminate the closest senator that s next to them
	- that's because they are the one who gets a chance to ban you or teammate asap
- so what we do is we maintain queues of positions of each senator based on position, then we iterate through the two queues and get the top two senators
	- if radiant appears before dire, dire gets eliminated
	- if dire appears before, radiant gets eliminated
	- the surviving senator is added for the next turn (position + N (size of the string))
- through this process, the queue that ends up with a single senator is the one that has the winner

# Merge Triplets to Form Target Triplet:
- key insight: no need to consider triplets that are larger than the target because if you max your result with that triplet, then you won't ever get to target.
- so go through all triplets that have elements less than the target, and merge them
- see if that equals the target

# Partition Labels
- maintain track of the farthest index we have encountered so far
- that's the limiting factor of how long each partition can be
	- when we reach the farthest index so far, we know that the letter cannot appear again so it must fit within that partition
	- a partition needs to contain all occurrences of a letter, so that can be tracked w/ the farthest right index of each character we have
- so we track the farthest index each character has existed
- and then we keep track of last partition index
- as well as the farthest index
- as we process characters, we max out farthest
	- this handles edge case of character appearing in the last possible position
- if we are at the farthest, we know this is the largest possible partition containing all occurrences of characters within 
	- so we add farthest - lastPartition + 1 (this calculates the length since farthest and last partition are inclusive)
	- last partition set to + 1 (the start inclusive, of the next partition)
	- the farthest is latest index of current character
- at end return result

# Valid Parenthesis String
- we keep track of the minimum unmatched open and maximum unmatched open
- if we encounter a "(", we know for a fact this is going to increase both minimum unmatched open and maximum unmatched open
- if we encounter a ")", we know for a fact this will reduce both since any opening brackets this is a for sure a closing bracket
- if we have an asterisk we can either add a opening parenthesis or close a opening parenthesis
- throughout this process if our max ever goes negative, that means we have too many closing parenthesis, so we return false
- we always make min 0 since that means we can take some asterisk and make it empty space
	- this is because min is the least possible, so we always force a closing
- if our max is ever 0, that means we definitely have more ")"
	- this is because max is the best case (conservative, only decrease when we see ")")
- at the end, minimum and maximum represent the full range of possible open bracket values that cant be closed
	- if 0 does not fall in this range that means we can't close at all, so we return False


# Candy:
- idea:
	- every child starts with a candy, so the candy array should be populated w/ 1s
	- a child that has a rating greater than both left child and right child gets more candies
- approach:
	- initialize 1 array w length of ratings
	- iterate from left to right, and within bounds, check if previous rating is less than current.
		- if so we need to have more candies than the left child (incrementally build up)
	- then iterate from right to left, and within bounds, check if the next rating is greater than current
		- if so need to have more candies than right child but don't violate the left condition (check the max)
- this guarantees smallest because this is incrementally building from a base of all 1s. 

# Unique Paths:
- can do dfs where dfs from row, column represents the number of ways to reach the final position
- if we ever go out of bounds, return 0
- if we get to the last position return a 1
- then we return dfs from 0,0
- but there's a lot of repeated subwork
- so we can cache results
- through memo
- represent the same concept with a 2d dp table
- last entry of 2d DP table is a 1
- then we go backward through each row and column and add the value of (r + 1, c) or (r, c + 1)
- then we return 0, 0 position

# Unique Paths II:
- can do dfs from where row, column represents the number of ways to reach the final position
- ever out of bounds return 0
- but if our current spot in the grid is a 1, return 0
- then we return the sum of of `dfs(r + 1, c)` and `dfs(r, c + 1)`
- since we have repeated work through overlapping subproblems, we cache.
- we can also tabulate our solution where we have DP grid, last item is set to 1 since there's only one way to get to the last place
- iterate backwards through rows, through cols
- then keep adding in bounds the above recurrence relationship (continue to next iteration if there's an obstacle)

# Minimum Path Sum:
- same spiel above as dfs and then memo
	- dfs: out of bounds, return float('inf')
	- if at the end, we return the grid value
	- then we return grid value + the min of r + 1, c and r, c + 1d
- dp table represents the minimum path sum from that position to the last position
- apply above recurrence iterating backwards through rows and columns
- return the first entry

# Longest Common Subsequence
- recurrence relationship:
	- `max(1 + dp[r + 1][c + 1], dp[r + 1][c], dp[r][c + 1])`
		- if two things match, we can pick that and then move on, or skip to either of the other words (we want to take the max)
	- `max(dp[r + 1][c], dp[r][c + 1])`
		- if they don't match, we continue only with one word
- if we have empty string and other string doesn't, 0 LCS
- create DP table
	- size of (text one  + 1) columns, size of (text two + 1) rows
	- then iterate backward through text two backwards (the rows), iterate through text 1 (the columns), and then do the recurrence relationship above (stays in bounds)
- return first entry of DP table

# Last Stone Weight II
* hard to get intuitively
 - Stone weight comparisons are just a chain of additions and subtractions
 - so what this means is we need to do is partition into a positive sum and negative absolute sum and then subtract the two
 - so pick and unpick strategy for half because we want to minimize this sum which can be done by half positive half negative
	 - so target is half
	 - then if we are at end of array then 0
	 - if our target goes negative, return float('-inf')
	 - then `max(stones[i] + dfs(i + 1, left - stones[i]), dfs(i + 1, target)`
- can memo this
- when doing dp table, create rows of indices and columns of targets
	- indices range from 0 to len(array) (len(array) + 1) and 0 to target (target + 1)
	- dp table being one extra position allows us to avoid checks
- then iterate through array backward (i) and target (t) backwards:
	- if the t - `weight[i]` is bounded apply above recurrence relationship
	- else make it `dp[i + 1][target]`
- then we know that the positive sum is the last entry in the first row
- the negative sum is the total minus the positive sum
- we return the absolute value of their difference

# Best Time to Buy/Sell Stock w/ Cooldown
- maintain state of if you can buy or sell stock at this point as well as index
- if index is last position of array or exceeds, return 0
- then if we can buy stock then we want max of move to next index or get a negative of the current index and go to the next index with the state flipped
- if we can sell stock then we want max of move to the next index or get a positive of the current index value and go to the next index (which is i + 2) with state flipped
- then run this dfs at 0,0
- if we have a table just create with bounds i + 2 index wise and then each row has two things
- memo for repetitive states, etc. 
- then just apply the recurrence relationship
	- we don't have to worry about bounds that way

# Coin Change II
 - if we our index ever exceeds the length of the array or the left value is ever negative, return 0
 - if the left is equal to 0 return 1
 - then for the particular coin we have the choice to take it and stay in the current coin only if in bounds`dp[i][a] += dp[i][a - coin]`.
	 - We also have the choice of just carrying over from the next index
	 - `dp[i][a] += dp[i + 1][a]`
- memo wtvr
- but for dp table, maintain columns of amount + 1, and rows of  i in range(len(string))
- each row and column tells us the number of ways to get to the target starting at that amount and index
	- the last column of the table should be all ones
- then iterate through all indices of the array
	- then all amounts starting form amount - 1 (since we don't want to overwrite the base cases)
	- to simulate going to the next index just add i + 1 position w/ the same a
	- to simulate picking a coin, check if adding to the current amount and the coin value is within bounds
		- if so just add `dp[i][a + coinAmount]`
	- return first element of two d array
# Target Sum
- to add symbols, its just seeing if you can add or subtract numbers to get the correct target
- so your dfs sees whether i is at the end and the sum you have is equal to target
	- return 1
- if we are at the end and then the sum isn't equal to target
	- return 0
- this is because we need to use all numbers
- then just do `dfs(i + 1, s + numbers[i]) + dfs(i + 1, s - numbers[i])`
- memo this
- for the tabular approach, since `s + numbers[i]`or `s - numbers[i]` can encompass a wide range, instead of having a preset table, use a list of dictionary
	- each list index represents the first i numbers
	- and we have information about the counts of all possible sums that can be made
- we initialize `dp[0][0] = 1`
	- this is because using the first 0 numbers and having a sum 0 can be done in only one way.
- iterate over entire array
- then for every single sum total pair in the corresponding dictionary
	- do `dp[i + 1][sum - nums[i]] += count`
	- do `dp[i + 1][sum + nums[i]] += count`
- then we return the `dp[n][total]

# Interleaving String:
- general intuition:
	- if the third string is less than the first two, return false
	- if we reach the end of the first string, then the rest of the third string starting from second string index + length of first string needs to equal the rest of the second string
	- if we reach the end of the second string, then the rest of the third string starting from from first string index + length of second string needs to equal the rest of the first string
- with these base cases, if the two strings in that position equal the third string's position, either could be picked so or with `dfs(i + 1, j) or dfs(i, j + 1)`
- else if only one equals, move ahead with that index
- else we return false because there's no match
- we can use recursion here, but there's repeated subproblems so we memo answers
- make a 2d dp table of columns of len(s2) + 1 and rows of len(s1) + 1
- for base cases, its the last row and last column of the table
	- iterate over the string that isn't complete and then see whether the remainder of the left string is equal to the third string with `i` + `len(complete string`)
- then to avoid overwriting the base cases, just iterate over the string indices
	- and then apply the above recurrence relationship
- so the positions in the table tell us if interleaving is possible from those positions
- return at 0,0
# Stone Game
- any time you have a situation where there's a game with two players, it becomes 3d dp if you maintain the left and right
- simplify by tracking the score difference for this player
- if the left and right pointers are the same that is just the pile
- since we can either take on the left or the right, we want to make the choice the maximizes the score difference so we do `max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1)`
- you can memo and such
- for the dp table approach:
	- create table of size len(piles) by len(piles)
	- have every instance where i and j are equal equal to the piles value 
- then we do interval based dp approach
- iterate over the length of piles 
- and then have l iterate backwards (from r - 1 to 0 because we calculate i + 1 before i)
	- for each position, calculate the max of `max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1)`
	- this prevents going out of bounds
- then we see whether the first entry is positive (indicating a positive score difference for the first person and they thus win)
	- return true
- else return false

# Stone Game II
- calculate the difference in score Alice gets
- then take the sum of all the stones and then we know:
	- alice - bob = difference
	- alice + bob = total
	- so alice = `(difference + total)//2`
- end of array, return 0
- then iterate from i to min(`2*m` - 1)
- add to the sum of stones picked, and then we dfs to (j, max(m, i -j + 1 (that's the number of piles picked)))
- then we get the difference by doing `dfs(0, 1)`
- then calculate the sum and and apply above equation
- because each index can be reached from multiple other indices, we memo our dfs of (i, number of piles)
- don't even bother with tabular for this not worth the confusion (revisit later?)

# Edit Distance
- if two words have the same letter, we move on
- when we have to delete we can do it from either string and advance character for deleted string `dfs(i + 1, ,j)` or `dfs(i, j + 1)`
- we we have to replace, we would replace it to induce a match and move both pointers.
	- same idea when we have a insertion (insert to force a match)
- so do dfs like this
	- base case if one is used, to edit the other you would just do removals to populated string or insertions into empty string
	- so the length of the string minus the index
- else apply logic
- can memo because we would have a lot of repeated states
- do dp grid
	- have rows, cols of size one more than string
	- and then do base cases of iterate through rows or columns while making one thing constant (last row or last col)
		- the length of the string - index
- then iterate backward through strings and populate table through above recurrence
- then return first entry of table
	- each entry represents minimum number of edits from that index to the end


# Longest Increasing Path in a Matrix
- we do dfs with a row, column, a previous value
- if the row or column is ever out of bounds or the present value is less than the previous or the `(r, c)` is in the visited set:
	- return 0
- if the row, column pair is in the memo, return the longest increasing path starting from that box
- else, store in the memo `1 + max(dfs(r + 1, c, current), dfs(r, c + 1, current), dfs(r - 1, c, current), dfs(r, c - 1, current))`
- then we return it
- and then we iterate through all rows, cols and then do a dfs
	- refresh the visited set (reinit.)
	- and then max out a variable w/ dfs
- return the max variable
- don't need a visited set
- this is because we strictly go from smaller to greater (there's no loop backs that way, its a DAG)

# Distinct Subsequences
- the idea is that we have to use up t within s.
- so our base cases is that if we reach the end of t, we return 1
- if we use up s but we don't use up all of t, we return 0
- if the two strings are equal at that character, we choose to take that character to use to make up t. but we could also skip and consider the next character
	- ```aab
	  ab
	  ```
	* so `dfs(i + 1, j + 1) + dfs(i + 1, j)`
* if not equal, we are forced to pick the next character to match to t
	* `dfs(i + 1, j)`
* then we return this
* we do repeated subwork so we memo
* if we make a table, just make the rows and columns one more than the length of the individual strings.
	* suppose that t is the rows and columns is j
* we iterate through the columns w/ the last row set
	* set that to 1, since we've reached the end of t
* then iterate through t backwards in the rows, iterate through s in the columns, and use that to populate the dp table 
	* bounds should always be fine since the dp table is one more than the size of the string
* and then return the first entry of the dp table

# Burst Balloons
- if we burst a balloon, there is chance that its left neighbor or right neighbor might get erased/eliminated
- so can't just naively go to the left or right bound 
- what we do is frame it as what happens when the balloon becomes the last balloon to get popped
- within an interval `[l, r`]: we know that treating balloons as last popped within this interval by iterating through as a variable `j`, we know the balloons popped before and after this interval is `dfs(l, j - 1)` and `dfs(j + 1, r)`. we can add these interval values to the immediately after calculating `nums[l - 1] * nums[j] * nums[r + 1]`
	- this is because we know the balloons outside this interval remain present
	- pop all balloons in this interval and then pick the last balloon to pop (multiply with things outside the interval)
- pad the input array with `[1]`
- base case is that if the interval ever becomes empty return 0 (so if l > r)
- then apply above recurrence relation, return that `dfs(1, len(nums) - 2)`

# Wildcard Expression Matching
- look at this later

# Path With Minimum Effort
- important idea:
	- when you pop from the min heap in Djikstra's, you know that that position (row, col) has had its minimum effort found
	- in BFS, because every edge has the same cost, the first you discover a node, that's already the shortest path
		- so we mark nodes as visited when we have found the shortest path
	- in Djikstra's discovering a node doesn't mean we have found the best path yet (only when pop since thats the smallest value so far)
			- so adding it to the visited set when seeing it is premature because there could be another path that is less that gets ignored because of the visited set
- dfs with a min heap:
	- heap has the starting position with a difference of 0
	- while the heap has stuff, pop
	- if the item is in the visited, that means we already found a minimum effort path (this existed earlier):
		- return
	- then check if its the end (rows - 1, cols - 1): if so return the min effort
	- then iterate over all the directions and then check if the dr, dc + r, c is within bounds
	- and then add them to the heap (we maintain the max of the existing minEffort and the subtracting the differences between the two consecutive items)

# Network Delay Time
- Djikstra's
- idea: minimum time to reach all signals is based on the largest accumulative time found throughout traversing the graph
- so:
	- create a directed adjacency list w/ the weights
	- then maintain a max time and a visited set and a heap
	- the heap should have the starting node
	- then keep popping from the heap while the heap has things
	- then if the heap popped is in the visited set continue
	- then max out the time with the time encountered so far
	- then add all neighbors and add the weight to the existing time
- then after this, return the max time if the length of the visited set is the number of nodes

# Min Cost to Connect All Points
- idea:
	- we want to always pick the smallest weight (cost) available
- one way (not optimal):
	- place in min heap the cost and the x, y
	- then while the heap has items, pop
	- if seen then continue
	- else add to visited set, add to total, and then iterate through all other neighbors, calculate the cost, and add to heap
	- `O(n^2 log n)`
- another way:
	- maintain a visited array and a distances array representing the smallest distances
	- maintain current node and number of edges and the total
	- since the number of edges for a MST has to be n - 1, keep doing the following while edges is less than n - 1
		- mark present node as visited
		- increment edges
		- maintain. next node
		- iterate over all points
			- calculate the distance to all other points from this node
			- update distances array
			- if the best distance to this node in i is less than the distance for the next node or there is no previous node, update it 
		- add to the total the distance of the best node
		- node becomes next node
	- return the total
- 