## Contains Duplicate:

- This is so trivial.
- Create a set, iterate through the list, if it’s in the set, then return false.

## Valid Anagram:

- Brute force: sort both strings, return true if they are the same.
- To sort strings, need to use sorted.
- “”.join(sorted(s))
- Optimal:
- Create a string representing the frequency of each character.
- Use ord(c) - ord(‘a’) to extract ASCII values.

## Two Sum:

- Brute force: inner loop and add everything.
- Optimal:
- We care about finding a complement. Have we already seen the value of that when adding up to this value gives us the correct answer?
- Use a dictionary to store these complements.

## Group Anagrams

- Brute force: sort and use that as a key, then append to the key in the dictionary
- Optimal:
- Use a string to represent the frequency of characters, and then use that as the key.

## Top K Frequent:

- Brute force: get frequencies, then put them on a heap, then keep the top k.
- Important idea:
- To maintain the k condition, after adding, you need to get rid of whatever is the smallest. So your heap should be a min-heap for removal.
- If you want the smallest, you should utilize a max heap. That way, u get rid of the largest value.
- Optimal:
- Bucket sort:
- Have frequencies as key, value pairs.
- Initialize buckets where the index represents the frequency and the items in the list represent the values.
- When we iterate backwards, we will, add items in each bucket till we have k. That way, we are considering the values that are the most frequent.

## Encode and Decode Strings:

- Brute force: just create some crazy delimiter out of columns and what not, and then split based on that.
- Optimal:
- Create a separator based on the length of the string and a “#” delimiter.
- Then, decode based on a two-pointer approach.
- I iterates the entire string, then j iterates over each part of the string.
- J finds a hashtag
- When it does, i moves to j, and j moves i + len of the string.
- Extract string through indices.
- Then, move i to j

## Product of Array Except Self:

- Brute force: nested for loop to multiply out every value that is not itself.
- Optimal solution: PREFIX SUM
- Calculate the product of everything to the left, then calculate the product of everything to the right.
- Prefix: assign in results, multiply.
- Postfix: assign the postfix * results[i], then multiply

## Valid Sudoku:

- Brute force: drop something about having a seen set each iteration and making sure that there are no repeats.
- Optimal: hash sets for rows, columns, squares that are defaultdict lists.
- Iterate through and then see whether the thing we are at is valid and if so, add it to the set. If its already there, return false as soon as possible.
- Hashing rows, cols are so easy. But squares are hard. Draw out a diagram and start mapping things out to. Get to (r//3 + c//3 * 3).

## Longest Consecutive Sequence:

- Brute force: essentially pick with every element, increment it, see if its in the list, then when it doesn’t max it out with a global variable.
- Optimal: convert our list to a hashset and only do operations if if there is no number before it.
- So literally just brute force with optimization of hashlist and the idea that there is no number before

## Valid Palindrome:

- Brute force: reverse the second one and then check that works out, but that won’t work because we are not considering spaces and everything.
- Optimal: two pointers, one starts at the beginning of the string, and the other starts at the end of the string. Move them towards each other till they overlap, but only increment pointers if its a alnum()
- Just remember that you call alnum on a particular character (s[i].isalnum())
- Additionally, ALWAYS CHECK BOUNDS SO YOU DON”T GO OUT.

## Two Sum II:

- Brute force: iterate through every single combination, see if it gets you to the sum, then return 1-based indices.
- Cannot use the same number by the way.
- Optimal: two pointers, one is at left, one is at right. If we get the target based on the numbers added, then return [l + 1, r + 1]. Else, if greater, reduce right pointer to get a smaller number. Else if smaller, left pointer to get a larger number.

## Three Sum:

- Brute force: sort the array, and then consider all subarrays of size 3, and then return valid ones after putting them in a set.
- Issue is that this is O(n^3)
- Optimal, sort and then use two pointer techniques for the subarrays.
- i is the outer variable, j and k are the inner variables.
- If nums[i] == nums[i - 1], then continue
- Else, initialize j and k to respective range of search space.
- Then, find the sum of nums[i], nums[j], nums[k].
- If > 0, then reduce right most pointer (k)
- If < 0, increase middle pointer.
- If = 0,
- Then, add to the results.
- Then, increment middle pointer as long as possible till not a duplicate is found.

## Container with most Water:

- Brute force: iterate over all combinations of heights with nested for loop. Minimum of whatever you find determines the height * (j - i).
- Optimal:
- Two pointers, one starts left most, one starts right most.
- You calculate the area by considering the smaller of them, and then max that with the result.
- To figure out which way to go, you are limited by the smaller one. So move the smaller one towards the center.

## Trapping Rain Water:

- Brute force: key observation is that the amount of water is determined by the minimum of the left-most max height and the right most max-height. Thus, what we can do is compute three arrays (one that finds the left-most max, one that finds the right-most max, and one that finds the minimum). Then, we know that the amount of water we can store is min of these - the current height but it cannot be lower than 0. Sum these all up.
- Optimal: we use two pointers, l and r, and associate a global max for both in terms of what we encountered. KEY POINT IS THAT WE ONLY MOVE THE POITNER THAT HAS THE LESSER GLOBAL VALUE AND DO WORK WITH IT BECAUSE THE AMOUNT OF WATER IS DETERMINED BY THE MINIMIMUM. After we do our result calculate, we update the respective global max.

## Valid Parentheses:

- Brute force: mention some bullshit about how we need to maintain the order of insertions so a stack make sense because of how things are ordered (run through an example)
- Optimal: Create a mapping of opening to closing parenthesis.
- Then have a stack:
- Insert opening items to the stack:
- If we run into a closing item, then, check whether the stack has something. If it doesn’t, clearly this is in valid. Return false.
- Else, pop from the stack. If this doesn’t match, return False
- Only return true if the stack is cleared because that means all opening parenthesis were matched.

## Min Stack:

- Brute force: iterate through the stack every time and try to find the min, or have a heap or something.
- Optimal: just maintain a second stack that always carry the minimum value it has seen so far. Updates it when the value we are looking at right now is less than whats on top, but else reappend.

## Reverse Polish Notation:

- Just realize that the pattern of processing follows a stack, where the first thing that you pop is equal to the second operand and the second thing that you pop is equal to the first operand.
- FLIPPED
- Next, realize that floor division rounds toward -inf, but casting a decimal division to an integer 0.
- ONly two things are popped at a time.
- Have a set with the symbols, if the token is in there, then pop last two from stack do operation, re add. Else add as an integer.

## Generate Parentheses:

- Not really a brute force way, just think about the decision tree.
- You can only add an opening if its less than equal to n.
- You can only add a closing if its less than opening.
- When u add something to the stack, you need to pop it. But don’t call dfs again because there is not another part of the search space to exist. We just want to revert the state space.

## Daily Temperatures:

- Question devolves to, what is the next greatest element.
- Can do a nested loop and figure this out.
- Optimal:
- Monotonic stack.
- We only insert to the stack if it is empty or the value we will add is less than what’s there.
- So our stack will always contain values less than something at index i.
- Thus we look for the greatest element than the max smallest, empty our stack.

## Car Fleet:

- Approach: essentially need to figure out whether the car is going to catch up to the car that is ahead of it via position.
- So when it does, you now consider the car that is the most ahead as the “combined car”.
- Replicate this behavior in a stack.
- We essentially add times needed to reach the destination for cars to the stack, then pop when what we add is less than what’s already there.
- Thus, the number of fleets would be what’s left on the stack.
- Create a pair data structure storing (position, speed), but sorted by position.
- Or you can create a fleet_time variable, where a new fleet forms whenever time is greater than the fleet time (because it will eventually end up there).

## Largest Rectangle in Histogram:

- Approach:
- Keep a stack of heights that can be extended to the right. Once we reach a bar the top of the stack cannot extend to the right, we keep finding the max_possible area, and set a starting_index for the bar that is the old_index SINCE THE NEW BAR IS SMALLER THAN WHATS TO THE LEFT AND CAN THUS BE EXTENDED IN THAT WAY.
- Then, we get a stack of candidates that can be CONTINOUSLY EXTENDED TO THE RIGHT.
- Find max from there.

## Binary Search:

- Approach: ts so easy

## Search a 2d Matrix:

- Brute force: go through and linear scan.
- Optimal:
- Binary search for row: just compare based on row first and last. If its greater than row last, we know we have to go bottom. Else, go top. Then binary search the target row (bottom + top)//2.
- Binary search whole thing: l = 0, r = m * n - 1. Get row via (m * n)//COLS and col via (m * N)%COLS. Then do normal binary search.

## Koko eats Bananas.

- Brute force: go through the answer space range (1, max(piles)), then find the first instance where it is <= h (bool T).
- Optimal solution: binary search the space range, based on the separation of Trues and Falses. If we have a false, we know we have to go all the way to the right, so adjust left pointer accordingly. If we have a true, that means we should record this result, but there is the possibility it is to the left, thus move right pointer accordingly.

## Find Minimum in Rotated Sorted Array:

- Brute force: just find this shit throughout the array.
- Optimal solution: find an invariant that is if its less than equal to the rightmost element. Then we know its a candidate, min it with what we have and then we can move to the left. Else, move to the right.

## Search in Rotated Sorted Array:

- Brute force: just literally search
- Optimal solution: binary search. Calculated your mid, and then see which side is sorted. If the left-most element is <= the mid, that means the left-hand side is sorted. See whether the target is within this range by comparing to the left-most and mid, if it is, move search to there, else move to the right. Do the opposite for the right-hand side. If it is sorted, then see whether target within mid to right. If it is, move search to there, else move to the left. Just think about it.

## Time Based Key-Value Storage:

- Brute force: Makes sense to have a dictionary where each key is tied to a (time_step, value). We can add normally. To get, we need to first see whether the key is present in the dictionary. If it is not, then we can return “”. Also check whether the required time-stamp is less than the first one, and if it is, we know it can’t exist so we return “”. Else, once we find something greater in terms of time stamp, we can return the one before it.
- Optimal: why not do a binary search on the setting side. If we find the exact time stamp we are looking for we can just return it. If what we find is greater, then shift the search towards the left. If we find something that’s less, we can update our result (which is initialized to an empty string) to the answer, and then try to find a larger true (shift search towards the right).

## Median of Two Sorted Arrays:

- Brute force: merge the arrays, and then find the median based on whether length is odd or even. If odd, just //2 the len, if even, do that but add the entry before and average.
- Optimal: very difficult to figure out. Essentially look for the smaller Array (A), then BS that based on a partition for it and the other array B (A + B + 1)//2 - smaller array partition. Then, get the maxLeftA, maxLeftB, minRightA, minRightB. If the partition ever ends up 0 or len(A) default to float(‘-inf’) for 0 and float(‘inf’) for len(A). If the crosses are less (max  < min) for both, we found the result. If this is even, we would take the maxes of the maxes and the mins of the mins, then average and return. If odd, we would take the maxes of the maxes. If our first cross maxLeftA >= minRightB. Max left A has too large of an element, so move search to the left, else move to the right.

## Best Time to Buy or Sell Stock:

- Brute force: nested loop, for every j within i, maxout the profit.
- Optimal: realize that negative profits don’t matter. Thus, have two pointers (a buy pointer, or a sell pointer). If the profit is negative, shift both pointers (i = j, j += 1). If it’s positive: get the result, and max it what you have.

## Longest Substring without Repeating Substring

- Brute force: check every substring possible in terms of no repeats. Have an outer i, then initialize a set for it. Then for each j from that i to the len(s), add character to the set. When we see a repeat, break from loop. Then, update the max.
- Optimal: sliding window: essentially have a l, r pointer. Expand r till a repeat is found in set, then keep popping s[l] and incrementing l. When we have a valid condition, update the max, and then add s[r] to the set, and then increment r.
- You can only add a new character after seeing it is safe to.

## Longest Repeating Character Subsequence

- Brute force: check every substring possible if the length of it - max of a frequency array has and keep updating max while its valid.
- Optimal: use a sliding window. We process the current character by incrementing its frequency and globally maximizing a frequency variable. Then, we see whether (j - i + 1) - max_freq > k. If it is, keep popping from the left and decrease frequency.
- In all dynamic sliding window questions, we need to first consider the character. In #30, this was an implicit consideration by seeing whether it was part of the set. We then add it to the set to mark it as processed.

## Permutation in String:

- Brute force: O(n^2) where there is an inner loop that gets each substring that’s of length s1 within s2. See whether that matches and return.
- Optimal: static sliding window. Steps:
- Initialize the smaller string’s frequency map.
- Initialize the larger string’s frequency map with l, r pointers, while r < len(smaller string).
- So l is inclusive, r is exclusive.
- Then, while r < len(s2), we get rid of s2[l], increment l pointer, add s2[r] BECAUSE WE HAD SET THE R POINTER TO BE EXCLUSIVE, and then increment r pointer.
- So when r >= len(s2), we have one last sliding window.
- Check that.
- Edge case: len(s1) > len(s2): obviously a bigger string cannot be in the smaller string.
- Find All Anagrams in a String
- Sliding window approach as above.

## Minimum Window Substring:

- Brute force: check all possible substrings and then do analysis of htem.
- Optimal: dynamic sliding window that expands based on whether what we need matches what we have. Have and need are manipulated during the inner loop, where need is initialized to the number of keys in t’s frequency dictionary. We increment frequency of s[r] pointer, then update the have based on whether s[r] was in the t frequency dictionary and if their frequency is the same. We contract while have == need, updating our min_length and our l, r pointers. Then decrease the frequency of s[l], and update have based on whether our frequency in s is less than frequency in t for that. L += 1, outside of this loop, increment r.

## Sliding Window Maximum:

- Brute force: you just find the maximum by iterating through every single sliding window and recording it.
- Optimal: use a deque. Essentially, start off by building the sliding window by popping from the deque if it exists and the last value is < than the present, then appending. Move right pointer. After, if the left pointer is less than the first element of the deque (INDICES ARE INSERTED TO THE DEQUE), then pop it. From there, we know since we have a monotonically decreasing stack, the left most value will be the index of the greatest. Repeat this process for the loop.

## SLIDING WINDOW TAKEAWAYS:

- Two approaches: static and dynamic:
- Static: prefill your window with a l and r pointer (r is exclusive). After you loop r < len(s), you have one last window to process so tackle that.
- Dynamic: l, r set to 0. Whatever other structure needed is also set. Then, process, contract, expand.

## Reverse Linked List:

- Just diagram it. You have to keep track of the next pointer. So what you can do is have a previous and current. Store the current.next in a temp variable, set current.next to previous, set previous to current, and set current = current.next.

## Merge Sorted Linked lists:

- Just use same logic as merging two arrays (have two pointers, pick one that is smaller). Have a head node for the new one, and a dummy. Then, make sure to return new.next because new is a empty node signifying the start of the node.

## Linked List Cycle:

- Brute force: hashset, dummy node, add to the hashset, if found return False end of loop return True
- Optimal: tortoise-hare algorithm, slow pointer, fast pointer (both initialized as head), while slow.next and fast.next and fast.next.next, move pointer then return false.

## Reorder List:

- Get middle of the list through slow and fast pointers.
- Detach the list.
- Then reverse the second list.
- Then, interweave them. Condition for this is always the shorter list (while next) This works because the second list is smaller. Then, do an interleave, where we get first.next and second.next.

## Remove Nth Node from End:

- Brute force: reverse the list.
- Optimal: left and right pointers. Move the right pointer till n > 0. Then, move l and r till r is end of list. Initialize previous to None. Set previous to l before incrementing l. We know for a fact l is valid. Thus, we need to check previous. If no previous, we at the head. Thus, just set head to head.next. Else, set previous.next to head.next.

## Copy List w/ Random Pointer

- Brute Force: two pass dictionary, store all copies in a map. Then, set random and next pointers. Return what’s in the map for the original.
- Optimal: interleaving. First, create copies next to the originals. Then, set randoms. Check if the present has a random. Then, present.next.random (copy’s random) would be equal to present.random.next (random’s copy). Then, decouple lists. Get copy, and then set current.next to copy.next. If copy.next, then set copy.next to copy.next.next (the next copy node). Then move current = current.next. Return new head stored.

## Add Two Numbers:

- This is hella light just gotta keep running loop with or, and then have default checks for 0 or none depending on the situation. Get answer modded by 10, get carry floor dividing by 10

## Find the Duplicate Number:

- Brute force: use a set.
- Optimal:floyd turtle rabbit algorithm, slow and fast pointers, where pointers are numbers that refer to each number (each value of the array is an index pointer to another index). Then, keep looping to find the location where they intersect. Then, initialize a slow pointer and then keep going till they intersect. That’s where the intersection point is. Return that. Some proof about p + c - x

## LRU Cache:

- Essentially need to maintain a hashmap with keys pointing to nodes.
- Node would have key, value, previous, next.
- Make sure to initialize.
- When initializing:
- Consider a left pointer, right pointer, capacity, and mapping
- Initialize left pointer to right pointer and vice versa in terms of previouses and next.
- Have add and remove methods that add elements to the right part of the linked list and remove from the left.
- Get:
- Return -1 if not found.
- Add/remove linked list node
- Return the value of the node
- Put:
- If its not there already, then create new node and and put it in hashmap and add it to the linked list.
- Else, update the value, and remove/delete
- If the size of the map is too large, exceeds the capacity, we know the LRU is going to be left of the left pointer. Thus, we delete it. Do so through removing from list and doing  del self.mapping[lru.key], as it gets rid of it from the cache.

## Merge K sorted lists:

- Brute Force: just merge two, then merge another one inside, merge another one inside (iteratively).
- Optimal: divide and conquer: merge two and store an intermediate; end of loop make intermediates the new one.

## Reverse Nodes In K Group:

- Optimal: such a BS question. Dummy node, attach next to head. Then get the kth node. After getting the kth node, if its None, break return dummy.next. Store groupNext. Then set prev, curr = kth.next, groupPrev.next. Then reverse normally. Store the groupPrev.next in a temp. groupPrev = kth, groupPrev = groupPrev.next. Return dummy.next

## Invert Binary Tree:

- Swap left and right child, then do the same of the left and right. Base case is when node is None.

## Max Depth:

- Base case of the node doesn’t exist or its a leaf node; in that case it should be a one.
- Else, get max of left and right and add 1.

## Diameter of Tree:

- Idea is that a diameter can be maximized by considering each node to be the center.
- Look at left_depth and look at right_depth. Sum of these would be a possible diameter.
- Then max it with another variable.

## Balanced Tree:

- Calculate height of left and right, see whether they are within 1. Use a nonlocal variable to store this.
- Then return the height. Always calculate the height, but you have a boolean flag.

## Same Tree:

- Base case:
- If there is no p or no q, then we return True (empty, trivial case).
- If p exists or q doesnt vice versa, or p or q doesn’t equal in value.
- See whether the current values match and whether the left and right would match as well.

## Subtree:

- Essentially check whether it's the same tree. For each node. So have a helper same tree function. Then, there a few edge cases:
- If not parent, then false. If not subtree, then true. Then see whether its the same from that point to the left or the right.

## LCA of Binary Tree:

- If the node is none, return float(‘-inf’). If the node is greater than everything to the left, than move to the left, and vice versa. However, if its greater than equal to one of them and less than equal to another, set that as the node.

## Binary Tree Level Order Traversal:

- The structure for level ordered traversals are the following:
- Initialize a queue
- Initialize a results list
- Add the root to the queue
- While queue:
- Initialize some type of layer processing, and determine the size of the layer
- Then iterate over the length of the layer.
- Pop a node
- Do whatever processing necessary
- If the layer processing structure is not None, process that

## Right Side View of Binary Tree:

- Literally the same structure as above. During the processing, always keep track of the node at hand, because that way we will always get the right-most node. Make sure to keep track of the actual node, not the value because if doing a right check, if the node is 0, it will evaluate to false.

## Count Good Nodes in a tree:

- Key takeaway: a good node is is one that is greater than or equal to encountered maximum so far.
- Thus:
- Have a current and maximum value in dfs. Set good to 0. If the current value > the maximum value, then increment good, then set the maximum value. Then, do dfs on the left hand side of the node and the right hand side of the node.

## Validate a BST:

- Key takeaway: you can bound the range of the BST. essentially going left, the node that you saw can be the maximum, and vice versa (it becomes the minimum).
- Thus:
- DFS shoudn’t return anything. Instead have a variable flag called valid initially set to True. Then, check. If the current node’s value is less than or equal to the minimum, and vice versa for the maximum, set that flag to false.
- Return the flag at theend.

## Kth Smallest Element in a BST:

- Key takeaway: an in order traversal gives you an ascending list of the the values. Therefore, if we keep going left, and start counting the nodes as we traverse nodes that are greater, we can find the kth largest element.

## Construct Binary Tree from Inorder and Preorder Traversal:

- Key takeaway: the first node of the preorder traversal determines the root, and everything on the left of the node in the in order traversal determines the left subtree, and vice versa.
- We have two pointers, a pre_index and an in_index.
- In our dfs, we define a limit. This limit is essentially determining what part of the in order we consider.
- Base case: if pre_index exceeds length of the preorder array, return. If the in_order value equals the limit, we increment in order to consider the other subtree and return.
- We create a node with the preorder index. Then, set node.left to dfs with node.val, and then node.right is equal to the existing limit (right can initially go forever).

## Maximum Binary Tree Path:

- Key Takeaway: a path consists of a node plus the best path on the left and the best path on the right summed.
- Thus, negative paths do not make sense.
- Therefore, we need to find the best left path by recursively calling it, and the same with right. Max with 0 in the case where the best node is the root.
- Then, we consider the path sum with the node at hand, maxing it with a global variable.
- Consider all paths going through each node.
- Then, we return a path up to the parent that considers the max of the left and right, then adds the current node up to it.
- This is because if we consider both the left and right and the current we no longer have a path.

## Serialize and Deserialize Binary Tree:

- Key Takeaway: makes sense to do a preorder traversal because we have a structured output of what to see (root, nested). So do a DFS to serialize, add to a result, then join with a “,.”
- Then, do a traversal on this string to deserialize. Have a global variable i that we use to read. If this exceeds or is equal to the length of the split array on “,,” then we return. However, if the array entry is “N”, return but increment i. Then, increment i. Repeat this process for the left node, and the right node return the node.
- WE DON”T INCREMENT BEFORE DOING DFS ON THE RIGHT NODE BECAUSE I HAS ALREADY BEEN PLACED CORRECTLy.

## Implement Prefix Trie

- Idea: the TrieNode data structure indicates whether a word ends as well as mapping to other characters.
- Thus, when inserting, we initialize a pointer to the main trie root. Then for each character in the word, we essentially see whether its in the mapping of the existing TrieNode. If not, create a new character-trie node mapping, then move to that Trie Node. At the end, we know the word ends.
- When searching, same process with a pointer. Then, if a character in the word doesn’t match, return false. At the end, we return whether there’s a word end.
- Finding prefix is the exact same process as searching, but we don’t care about whether its a word end so we return true.

## Design Add and Delete Word Data Structure:

- Idea: same operations of adding to a trie. However, search involves a DFS because of wildcard operations. DFS function has is (node, i) When a wild card is encountered, we run dfs on all children nodes and with index + 1 to see whether there’s a match later. If there is, we return True. If no children provide a match, return false. Else, do normal search operations where if the character not in node.mapping, return false. Then, move mapping to next node. At end, return node.word_end

## Word Search II:

- Idea: need a quick way to see whether a letter is part of a list of the words. Use a trie (initialize add with “self”). Add all words to trie. Do normal procedure with setting up rows, cols, visited set, etc. dfs takes in r, c, node, word. Invalidate and return when r, c out of bounds or if the board[r][c] not in node.mapping or if its in the visited set. Then process by adding (r,c) to visited set, then moving the node to the board[r][c]. If we reach end of word, that means we can add it to the words. Then, dfs, then undo (so get rid from visited set, and pop from words). Run this for each row and column of matrix. Important to note words is a set, since no guarantee we can’t generate the same word twice. Thus cast to list.

## Subset:

- Idea: pick or not pick. If picked go to the next one. If not picked go to the next one.

## Combination Sum:

- Idea: we want to decrease our sum all the way to 0. If it ever hits negative or the index in our list exceeds the length of the list, return. If target == 0, add a copy of our list to the results. If we pick a target, we have the ability to repick, so keep i index at that position. Pop the result, and then increment i if we want to completely not consider an element.

## Combination Sum II:

- Idea: we don’t want duplicates, so we can sort in order to get duplicates adjacent to each other. DFS format is the same as combination sum. We have the base case when the target == 0. But we have a loop that goes through j between i and the length of the candidates. We skip duplicates if j > i. Then, if the current value would make target < 0, then we break the loop because we have gotten too far. Then, we add the candidate to our answers, dfs(j + 1) to the next option, and pop.
- We don’t explicitly handle the dfs call to the skip because the loop takes care of that

## Permutations:

- Pick up the leaves of a full dfs of the nums space. So, our dfs terminates when the size of our result is equal to the size of results. We then, go through the nums, and if our n is not in the result right now, we consider it (exploring a new branch), and then dfs of the next index (next number) and then pop to undo and explore another branch.

## Subset II:

- Everytime there is a duplicates situation, need to loop and consider the next possible candidate.
- Thus, we do the classic logic for subset, but we keep incrementing j till we reach the last duplicate, and then we do dfs on that (j + 1).

## Word Search:

- Why do we need backtracking: possible that from one position, dfs doesn’t yield solution, but from another position, some of the cells visited from the first position comprise of the solution.
- So dfs, but we have an additional variable i that is used to determine what tposition of the word we are at.
- Our base case checks whether we are out of bounds, visited the cell in this loop, or the board value doesn’t match the index of the word we care about.
- If so return false.
- If we are at the last possible index of the word (len(word) - 1), return True because we have seen the entire word.
- Then, add to the visited set.
- dfs  all sides, pop from visited.
- Run dfs on all r, c combinations, return if true.

## Palindrome Partitioning:

- Pick/unpick doesn’t matter because you are looking for whole sections of a string, rather than individual characters.
- Thus, approach would be to iterate over all possible substrings from a given i, and if its palindromic, consider it a valid partition, then consider partitions of the next string, and undo.

## N-Queens:

- Pick and unpick logic. Essentially we have multiple sets. A column set, a positive diagonal set, a negative diagonal set. We also create a board. Our dfs goes through each row and has a count of how many queens. If our count hits n, we make a copy of the board, and add it to the results. We then iterate over all possible columns, and see whether we are the visited set. If so, we continue. Else, we add it to the sets (r +c) for positive diagonal, (r - c) for negative diagonal, and the column. We set it to “Q”. Then we do dfs by testing the next column. We then undo it.

## Kth Largest Element:

- Brute force: reverse sort and return the k - 1st index element.
- Optimal: use a heap. Heapify initial input, and pop till the size of self.nums is k. Because that way, the top of the heap is the kth smallest element. Then, when u add a value, you do the same logic.

## Largest Stone:

- Just simulate it with a heap. NEed to find the max, so use negatives, pop the heaviest and second heaviest. Then, literally just do the logic thats described while length of it is > 1, and return the remaining element

## Kth closest points to origin:

- Brute force: sort but in reverse. Or heapify, and pop the kth if we have a max heap.
- Optimal solution:
- Run kth select. Kth select has a partition function where we want everything for a random pivot. Move it to the right. Then have a pointer that will store everything that is less than or equal to the thing. Iterate over based on euclidean distances. If it’s less than or equal, swap the value of the j and placement pointer. After, swap the right most with the placement pointer. Return this index that was randomly selected as a pivot.
- Then, we have left and right bounds. Run a binary search. These bounds are changed based on the random pivot provided. If we our pivot == k, we found exactly the k smallest on the left, so we return. If our pivot > k, that means we found more than k smallest, so we reduce search space. If pivot < k, then find more.

## Find Kth largest number:

- Brute force: sort, heap, same shit.
- Optimal solution: makes sense to do a three-section partition. One section is everything strictly less, one section is equal, one section is larger. We do the standard procedures for partitioning, but we also update a last_index when there is something that is less. So the actual swapping logic occurs when there’s a less than or equal to, but when strictly less we do an update. For the recursive call, there are three situations:
- We know there are three sections [l, lastIndex], which is everything thats less [lastIndex + 1, placement] which is everything thats equal, and then there’s [placement + 1, end]. If our k, which is actually len(nums) - k, since we partitioning in increasing order is greater than the placement, we know that we need to shift our search to placement + 1 area. If our k is greater than lastIndex, we know we found the kth largest, Else, we do the search into l, lastIndex space.

## Task Scheduler:

- Use a heap and queue.
- Observation is that u need to always pick the task that is most frequent.
- Thus, the heap would consist of remaining counts, queue would consist of items that are cooldowned and when they can be reassessed.
- Our algorithm would have a max-heap.
- Process:
- While the heap or queue exists. We increment a time step. If there’s nothing in the heap left to process (everything is in cooldown), fast forward to the earliest timestep. Else, if the heap has something, pop it, add one to the current, and then add to queue if the count doesn’t become 0.
- Then, if queue and the time of the queue matches the current timestep, we can readd it to the heap.

## Design Twitter:

- Very complex.
- Need to have three global data entries;
- Time (self.time)
- Following list: defaultdict(set) (this is because we need a really easy way to see whether someone is following)
- Tweet list: defaultdict(list) (enables us to easily access the last/most recent tweet)
- For the follower additional and removal, straightforward. Add to a follower ids following list, and check whether its present.
- Then, for creating tweets, decrement self.time, and then add to the tweet list.
- Hardest part of this is get news feed. The intuition is that we need to get the most recent tweets for the followers of someone. Important to ask whether someone is considered their own follower. We iterate over this, and if a follower has tweets, we find the last index of the tweet. Append the corresponding time, tweeid, followerid, and nextIndex to take a look at (last_index - 1).
- Then, we heapify this.
- We then keep popping from the heap while the heap exists and the len(results) < 10.
- We pop, add the tweet to the result. We then see whether the next index provided is valid. We then add that to the heap using the same process.
- Edge case checking: make sure to see whether follower is in following, whether index is valid, whether the follower has tweets.

## Median of Data Stream:

- Brute force: can always sort and do whatever.
- Optimal:
- Important to note that in a sorted array, the median is easily either the greatest element on the left side of an array, the smallest element on the right side of an array, or average of the two.
- We can emulate this with a max heap for the left hand side of the array, and a min heap for the right.
- We insert elements in the max heap. If this changes whether the max heap’s max is > the min of the min heap of the right, we move the max element over. We have a strict ordering that the max_heap’s max is always less than or equal to the min heap’s min.
- We make sure the array is partitioned so that each heap’s length is within 1 of the other.
- If it’s not, we move the appropriate element (the max of the smallest, or the mins of the largest) to the other heap.
- If the length of the two heaps is the same, you would return the average of the max of the smallest + min of the largest).
- Else, you would return top of whatever heap is larger. ‘

## Number of Islands:

- Depth first search: straight forward.
- Breadth first search: need to have an add function for validation.

## Max Area of Islands:

- Same idea as number of islands. But phrase recursive DFS as an area accumulator. BFS would require you to consider each addition incrementing the result, and then u would return result.

## Clone Graph:

- Essentially have a mapping from old node to new node. We have a dfs where if we already found a duplicated node, we would just return it. Else, we would create a duplicate for the present node, loop over all the neighbors, and create duplicates for the neighbors and add them to the ne wnode’s neighbors. Return the new node at the end.
- And then return the duplicate.
- Watch out for edge cases like graph is empty.

## Walls and Gates:

- Multisource BFS: need to first get all of the gates, and then each node in the “layer of the bfs” gets.
- So, we get all gates and add them to queue and visited.
- Then we have a add function that will add valid entries to the q and visited set and updated their value to the layer requested.
- Then we do a normal BFS where we need to pop inside the loop.

## Rotting Oranges:

- Multisource BFS. This time, we have two conditions for continuing the while loop.
- We would first go through the grid, and add the rotten oranges while maintaining a count of the fresh oranges.
- We then have an add function that invalidates positions empty or out of the grid, then decrements fresh, adds to the queue the value that is not rotten.
- Then do a bfs while q and the number of fresh > 0. We pop, and do add function. If we have exhausted the queue and fresh still remains, that means not possible for everyone to be infected, so return -1. Else, we return 0.

## Pacific to Atlantic:

- Approach: essentially we need to see whether a cell can be reached from left and top as well as bottom and right.
- Brute force: we have a dfs for every single cell where we see whether r < 0 or c < 0 (Atlantic) and r >= rows and c >= cols (Pacific). However, this can be tedious.
- Alternative approach. Only run DFS on the outward cells and which cells inwards can the ocean reach. Do this twice (one for pacific, one for atlantic)  and find the set intersection.
- Important to note that in the DFS, one of the conditions is that the previous is smaller than the current. This is because we are thinking the opposite way. For water to reach the outwards bounds, the inner cells to outer cells path has to be decrease. So our current as we traverse must be greater than our previous.
- Need increasing paths, decreasing paths would imply water can’t flow.

## Surrounded Regions:

- What is the question asking? There are X’s and O’s. We need to find all regions of Os that can be surrounded (so it has X in all directions).
- Solution that doesn’t work: find all Os in regions that are not in the borders of the board. Then mark them in a different value. Then iterate over the board, and make them Xs.
- Solution that works: do the reverse. Essentially, do a DFS over the borders starting at Os to see which regions cannot be surrounded. This is because any region that can reach the edge cannot be surrounded. Mark these a different value.
- Then, we can iterate over the board. All Os that remain are able to be surrounded, replace em. All things of the different value are reinstated to Os because they cannot be surrounded.

## Course Schedule I:

- Classic cycle detection question. Approach involves having a visiting set and a visited set. Visiting set maintains track of what we have seen in the path so far, visited set represents all nodes completely processed (confirmed acyclic)
- We create an adjacency list (defaultdict(list)), and then we add course to prereq’s list. We then do a dfs. If we have seen the course in out visited set, that means its safe and its acyclic. If we have seen the course in the visiting set, that means in our current dfs process, we have encountered this course so therefore we have found a cycle.  We then do a dfs on all neighbors. If any of them return false, we have found a cycle later on in the dfs, so we return False. We then remove the present course from the visiting set (as we have completed a full dfs from that point), and add it to visited. We can then return True at the end.
- We then need to run this DFS on every single potential course.

## Course Schedule II:

- Same idea as above but we need to track the path. So we have appends to output after all successors are taken care of. So we have [successor 2, successor 1, predecessor]. So we would then run dfs on every single possible node in order to account for disconnected components. The path we get is ordered in the way mentioned above. So, in order to get the correct order, we need to reverse.

## Graph Valid Tree:

- Question boils down to finding a loop in an undirected graph. So, adjacency list needs to account both ways.
- Also, we run into this edge case where when we run a a DFS, we will always head have our parent as a neighbor. This would cause a false positive in the logic. So we have a previous node that tracks where we came from. When doing a DFS across our neighbors, if our neighbor is equal to previous, we continue.
- We then follow normal cycle detection approach. We only run the dfs from the first node since a tree is a single acyclic component. We also check whether our vsitied set is of the length n since that means all nodes are processed and the tree is complete (prevents a forest).

## Number of Connected Components in a Graph:

- DFS: this is simple. Have a simple DFS that does the parent node accounting and maintains a single visited set. Then, run this over all the components, and increment a result variable. Return this result variable.
- Union Find Algorithm: maintain a series of ranks and a series of parents. Ranks essentially determine tree height. Find operation is done recursively: if the parent is not equal to x, its parent becomes self.find(parent). At the end, return parent. Union: get the roots of x and y. If they are the same, we are in the same set, so return False (a union cannot be performed). Else, if the rank of the root_x > root_y, we know we should merge root_y into root_x’s tree, so root_y’s parent becomes root_x. Vice versa applies. If equal, u can do an arbitrary merge and add one. Then, for count components, we just try to merge all the edges and decrement the number of individual nodes each time. The result is the number of components.

## Redundant Connection:

- We use union find, but because this is one-based we do parents and rank with a +1 adjustment.
- All our traditional union find operations exists. But for each pair of edges, we try to merge them. In a normal tree, this would be successful, but when we reach a merge that returns false (because the nodes were already connected), we know we have found a cycle, so we return that pair.

## Word Ladder:

- Basic BFS to see whether we can reach from a start word to an end word.
- Condition is that there can only be max one letter change.
- So, construct all wild card patterns that one letter change can allow. For example: cat → *at, c*t, ca*. Essentially, we are seeing what the rest of the characters must be. If two words have a common pattern formed, that means there is a one letter change between them.
- To make the adjacency list, each pattern is a key, with a list of words that follow that wild card format.
- We then do a traditional BFS, but we have our counter variable set to one initially. We then pop a word, if the word is the end word we return the counter. Else, we formulate each possible wild card pattern, and fetch the neighbors. If the neigbors are not in the visited set, then they are added to the visited set and queued up as well.
- Forming wildcard: pattern = word[:i] + "*" + word[i + 1:]

## Climbing Stairs:

- So we know that we can make one step or two steps at once. So each level’s ways of getting to is determined by the total number of ways we can arrive at the level before, and the level two stairs below.
- Recurrence relationship: ways[i] = ways[i - 1] + ways[i - 2]
- Recursively, literally just coding fibonacci. We have base case of i = 0 (trivia case, no stairs so only one way), i = 1, first stair only one way to get there. We then apply recurrence.
- Do memo to keep track of what was calculated to beat TLE.
- DP, same concept. But the DP table is n + 1 because we need to address the 0th stair situation.

## Min Cost Climbing stairs:

- So we know we want to minimize the cost. So for each step, we take the current value, and then take the minimum of the next step and two steps next and there. These values represent the total cost of going to the end from there. Thus, the recurrence is dp[i] = cost[i] + min(dp[i + 1], dp[i + 2]).
- Our base case is when we go over the stairs. We return 0.
- We then take the current cost and then take the minimum of the next possible moves till the end.
- We then run this dfs on the 0th index and the first index to see the min possible cost starting from each place, and then return the minimum.
- So for the backtracking approach, add padding. Iterate over cost, but make the DP array two more. So that we can use the recurrence cost(i) + min(dp[i + 1], dp[i + 2))

## House Robber:

- There exists two approaches. We can take the current house, and then we add whatever the max loot is for the house that’s two away (not immediately adjacency), or we can avoid this directly and take the next house’s max loot:
- max(nums[i] + dp[i + 2], dp[i + 1) → recurrence relationship.
- So we can do the typically recursive approach with a base case of 0 when exceeding the length of the list.
- And then we can apply memoization.
- In the DP case, we can work backwards since we need what’s ahead. So we add two more items to the dp matrix, and simply iterate backwards on the nums array and populate DP. We can then return what’s in the first entry.

## House Robber 2:

- Use the circular restriction to run two separate linear cases. If we can’t pick the first AND last, try to see the max of picking the first and excluding the last and excluding the first and picking the last.
- So dp becomes 2d: start, end.
- Normal house robber logic, but start + 2 to pick and move to the next house, start + 1 to skip the house.
- Then max dfs([1:], [:-1])
- Typical memo strategies.
- For the DP approach: have a helper function that takes in a specific array. Then pass in the splits mentioned above.

## Longest Palindromic Substring:

- Brute force: go through every single substring, and update global variables based on length. Remember that a one character substring is a palindrome.
- DP option: this is so unintuitive. (i, j) represents range. If i > j return a blank string. If they are the same return the character. If i == j and everything before is a palindrome, we return this. Next, we calculat ethe left and the right (i + 1, j), (i, j - 1) (since we are starting from 0, len(s) - 1).
- Best option: centering out. What if we treated each character as the center of a string? That way, we can expand outwards while the outer variables match, and we update our results. This only accounts for odd length palindromes however. So we set our l, r = i,  i + 1. We do the same process

## Number of Palindromic Substrings:

- Same approach as one above. But this time, we need to count. Brute force is similar, centering out is also similar.
- DP:
- Base cases: single character, two characters as well. (i, j) has to be a palindrome if (i - 1, j + 1) is a palindrome. Essentially, keep track of the numb er of palindromes through Trues and increment count.

## Decode Ways:

- Recursively, the base cases are the following. If we reach the end of the string, we have gotten one successful encoding. If we exceed it or the character we are at is a leading “0” we return 0 since that means we haven't found a way by going too far or the leading value is a “0” (not allowed).
- Then, we consider picking the single digit by doing ways = dfs(i + 1). We then consider the next digit (two digit) by seeing whether its in bounds as well as whether its within 10 <= 26. In that case we can consider it a case, and then move th pointer to i + 2 (since i + 1) has been accounted.
- Memo is easy.
- DP table, add a padding of 2. Need to continue when we have a leading 0 but the same logic as before applies.

## Coin Change

- Always cache as little stuff as possible.
- 2D approach: index and amount. When amount is negative or index exceeds length of list, we return float(‘inf) since no coins can help. When amount == 0, we return 0 since no more coins help. Picking this coin is a 1 + dfs(i, amount- coins[i]). Not picking is a dfs(i + 1, amount). We find min and return. Memo is easy, 2DP DP table requires filling right side bottom up.
- 1D approach: just keep track of amount. When amount < 0, return float(‘inf’). If amount == 0, return 0 since no more coins can help. Then, we have a result to find the number of coins for this particular case. We then iterate over all the coins, and we update res with the minimum of not picking it (res) and picking it (dfs(res  - coin)). We then return this.
- This is because the decision tree has the ability to pick every single coin.
- The memo reasoning: we create a list of all possible amounts + 1 (since we are 0 based). Each index represents the minimum number of coins needed to produce that amount. 0 obviously requires 0. We then iterate of the rest of the dp table. We have a result. For each coin in the coins, if we can use this count to get lower but not below 0, this is valid. So we can see whether we can get less possible coins to make this amount by considering 1 + dp[amount - coin] (that is, this coin plus the number of coins needed to get to amount - coin).

## Maximum Product Subarray:

- Brute Force: literally find every single subarray, and max out a result.
- Optimal: notice that the current minimum so far can become the maximum if we multiply with a negative number, and vice versa for the current maximum. Additionally, the current value itself can be the current maximum and minimum.
- Thus, we start off by setting the result, current minimum and current maximum to first element. We then iterate over the rest. We then consider extending both the current minimum and current maximum with the next element. Because of behavior with negative numbers and how the current minimum can become the current maximum, we then do the minimum of both extensions, as well as starting from fresh with the current element. Similar logic applies to max. We then max with the result.
- As with all contiguous stuff, doesn’t make sense to use direct DP.
- No typical reset factor because we need to consider all possibilities.

## Word Break:

- Recursively: Our base case is when our pointer reaches len(s). That means we were successfully able to partition. Else, if the pointer is greater than equal to len(s), we were unable to partition because we overstepped We return False. For our current i, we see whether we can form a partition from there. We have a variable that tracks this. We then iterate over every single possible word. We see whether the length of the word plus our i is within the range of s (len(s)) and whether the section of the string matching the word length [i:i+len(w)] matches the word. In that case, we dfs to the next available place (i + len(w)). If this is successful, we know a successful partition is possible, so we set our variable and then brek. We then return.
- Memo is straightforward.
- DP requires a [0] * (len(s) + 1) because its 0 based. We set the last most possible place as True, as when we reach that, we know we have successfully partitioned the string. We then iterate backwards over the string. We iterate over each word. If it falls within the length of the string and the partition matches the word, the value of the dp table is equal to the value of the dp table at i + len(word). Once we get a true, we know a partition is possible, so we break.

## Longest Increasing Subsequence:

- 2d Approach: pick/unpick option where we have a choice to pick this element if and only if its greater than what was picked previous. So given (prev, curr) we get the max of (prev, curr + 1), 1 + (curr, curr + 1). However, memo-ing this causes space complexity.
- 1D Approach: we need to create longest increasing subsequence from each particular index and then find the max across. So our base case remains the same (i == len(s), return 0). LIS starts off at 1. Now, for every single option in the future, we need to find the best pick. This is attained by doing LIS = max(LIS, 1 + dfs(j), as we need to attain the next element). We return this LIS. We then loop over al elements, and find the longest increasing subsequence.

## Partition Equal to Subset Sum:

- 2d Approach: we can essentially start off by filtering off sets where the sum is odd. No way to get this. If the sum is even. We can then do the pick/not pick technique. We just need a series of picks that lead to half the sum. So we have a i and amount variables. If we reach len(s) and the amount is equal to the target, return True. If we reach len(s) and the amount is greater than the target, return False. Else, we need to return picking (dfs(i + 1, amount + nums[i)) or not picking it (dfs(i + 1, amount)).
- 1d: we can simply find the number of sums that could exist by picking each element starting from the back. The DP data structure thus becomes a set. If we ever find the target within this set, we return True. Else after iterating backwards, return false.

## Insert Interval

- We can only insert the new interval if it’s end is before the start of the existing interval we are at. If the new_interval’ start is after the end of the current interval, can’t insert it yet. Instead, insert the current interval.
- In any other situation, we have a conflict. We handle htis conflict by merging into the external interval. Do so through minning the initials, and the maxing the ends to get the largest possible interval.
- Outside of our for loop, we know that the interval hasn’t been added yet. So we add and return.

## Merge Intervals

- Given last interval of [a, b] and an interval [c, d], these overlap if c <= b. Assuming that we have sorted all the intervals based on the starting time, the merged interval becomes [a, max(b, d)]
- Our results start off with the first interval. We then do the above logic. If there is that overlap condition, go ahead and update the last result with that [a, max(b, d)]. Else, normally add the interval.
- Use lambda function to sort: intervals.sort(key = lambda x: x[0]).

## Nonoverlapping Intervals

- If we have two intervals that overlap, we should pick the one that ends earlier greedily to minimize the chance of overlap.
- If we sort the intervals by ending, this makes our job easier.
- If we don’t have an overlap. We should update our comparison point to the latest interval. This is because if a future interval doesn’t overlap with the latest, that means it didn’t overlap with one before. But the opposite doesn’t apply.
- If our intervals has just one item, return 0.
- Else use two pointers. If overlap, j + 1 (since intervals[i] is guaranteed to be earlier. If two intervals don’t overlap, i = j, j += 1 (update most recent interval).

## Meeting Rooms I:

- Simple logic. Sort by starting, do the whole merging check. Remember the edge case of a single interval (True regardless).
- Good clarification question regarding inclusivity of a meeting time.

## Meeting Rooms II:

- Need to keep track of total overlap at any point. Thus, previous approaches cannot make sense because there’s no way to keep track of what already has happened w/ just two pointers.
- Simulate going through time. Sorted start and end arrays. If the start is less than the end, that means that a meeting has started. Thus, increase the count of the meetings, max it with a result, and increment start_pointer. If the end is greater than the start, it means that a meeting has ended. So we decrement count and move the end pointer. This way, we have a count of meetings at each time.

## Minimum Interval to Include Each Query

- Brute force: sort all the intervals, and then run through each of the queries, and iterate over all the intervals. First interval the point is on, return value for that.
- Optimal: use a min-heap and a pointer to keep track of what interval we are at. We sort queries as well to process in a natural time order. We use a dictionary to map what query is to what result. First, we go through each of the intervals while our i < len(intervals) and the start of the interval is less than the query (indicating that the query could be present within this interval). We then add these to a min_heap in the [length, end_point] format. After we consider all possible intervals that make sense, we then get rid of intervals that no longer make sense that were carried over from the last query by popping from the minheap if the end date (heap[0][1]) is < than the q. We then set the value of q in the dictionary the top of the heap’s length if heap still has values, else -1. Then, we return [dict[q] for q in queries).

## Maximum Subarray

- Brute force: go through every subarray, find the sum, return that.
- Optimal: Kadane’s algorithm. Strat is to have a local running sum. Initialize local and result to the first element of the array. Then, iterate over the rest of the array. At each index we have the option to restart or extend the local w/ the current element. Pick the max, set that as the local, set that global to the max of that.

## Jump Game:

- Brute force: DP with base case of len(nums) - 1 suggesting we can reach it. Then, doing dp on every i + j + 1 (since ranges are 0 based) combination. Then applying memo.
- Greedy: we have a goal set up at the last point. We iterate backwards starting from the second to last index. Nums[i] is the max jump we can have. If i + nums[i] >= the goal, we set the goal to i, since we know that the goal is attainable from this index. We keep doing this. At the end, we have created a chain of moves that enable getting to the end if the goal is equal to 0. This means that we were able to move the goal everytime we hit the last goal.

## Jump Game II:

- Brute force: DP with finding the minimum. If we reach the end we return 0 (no more moves). If we exceed, return float(‘inf’). Then you try every single possible through a dfs, return minimum through the whole min(res, 1 + dfs(i + j + 1).
- Apply memo
- Optimal: consider a BFS starting from the first place. Essentially we need to count the number of layers, with each “layer” representing a move. We know the end of the layer is the max we can go starting from the current. Thus, we can initialize two pointers (l, r). Outermost loop is while r is less than len(nums) - 1 (to ensure that the sliding window contains the last element). We maintain farthest variable. We then iterate over the range of the current “layer”, and identify the max index we can reach. Once we do, we move l to r + 1, and then set r to the max_index. We then increment our count. Return count at the end.

## Gas Station:

- Automatic knowing: if we have less gas than cost no way to do a circuit. Return -1.
- In that situation, we are guaranteed a solution.
- We only care about the difference at each position.
- So let’s have a start variable and a total variable. We iterate over all the stations and add the diff to the total. If the total ever drops below 0, the start index doesn’t make sense. So we need to move the start pointer. Cannot move the start pointer to anything between [start, i] because that would cause the total to be even lesser. That’s because:

- So we want the start index that ensures that the total never drops below 0.

## Hand of Straights:

- We automatically know that if the size of the hands are not divisible by the group size, we can’t divide so return False.
- Greedy Idea: we should try to form groups with the minimum possible value, to maximize the number of consecutive groups we can make.
- Idea: we need to keep track of the frequency of all the elements, as well as the minimum element available to us. To get the minimum element, we can use a heap. We can heapify the keys of the dictionary.
- While we still have minimum elements to pick, we pick the minimum and then reduce the frequency of it. We then have a i pointer that starts at one to keep track of the number of elements in our current group. While i < the number of elements permitted in a group, we see whether the minimum + i (the next consecutive element) is within the frequency map with a frequency greater than 1. If not, we immediately return False since a consecutive group cannot be formed. Else, we decrease the frequency of minimum + i element and increment i to indicate that an element was added to the group.
- Then, we get rid of all elements that have a frequency of 0 in the min heap to reach the next element that is valid.
- At the end if min heap is empty we have no gaps in terms of being consecutive. Thus, we return True.

## Merge Triplets to Find Triplet:

- Key observation: for a triplet to occur, each value of the triplet it needs to be in
- One or more of the triplets.
- Another observation: if a triplet has a value that is greater than the target value, we should never consider it, because it causes the formation of a triplet that will never match the target.
- With this, the triplets we consider have a, b, cs that are less than or equal to the target. Thus, the max will always yield the target. So we just need to see if the target exists.

## Partition String:

- Key Observation: we can keep extending a string to generate the maximum possible partition till we get to the last-occurrence index of a character we have encountered so far.
- Why is that? That means we have a repeat.
- So what we do is we get a hashtable with last occurrences of an index.
- We then utilize a sliding window approach. We keep track of our endpoint by maxing it with the endpoint and the last-index of our current character to keep greedily sleecting a larger partition. When we reach this, however, we add (start - end + 1) to the result, and we move start to i + 1 to indicate a new block.
- Need to keep track of size of contiguous ranges of arrays: use sliding window.

## Valid Parenthesis String:

- DP: pretty straightforward decision tree. We keep track of present index and then the number of open parenthesis. If this open parenthesis count ever goes negative, return False. Else, if we reach the end and the number of open parenthesis is not 0, return False. Else if it is 0, return True. Result = False. Try each option. If we have a hard coded open or closed, we have to try that specifically and increment open or decrease open. If we have a wild card, we try each option, as well as the wildcard.
- Optimal: we need to keep track of the minimum number of opening parenthesis and the maximum number. If we hit a “(“, both get incremented. If we hit “)” both get reduced. IF we hit “*”: we pick between “(“ or “)”, minimum gets decreased, maximum gets increased. At any point during this process we need to see whether the max is negative. In that case we can automatically return false. If minimum number ever hits negative, cap it to 0 cuz we can’t have negatives. At the end see whether this is equal to 0, because if it isn’t that means something was left unmatched.

## Single Number:

- Brute Force: use a hashset and do two pass solution where you count frequencies, and see which one has a frequency of 1.
- Optimal: use a XOR. If you XOR two things that are the same, it return 0. Order of XOR also doesn’t matter. Same bit xor gives 0, different, xor gives 1. If we xor a number by the same number an xor it again we get the original number. Keep xoring a result and if you will get the single occurring number.

## Number of 1 Bits:

- We can access the last bit by modding by 2. We can also get the next appropriate bit by shifting by one to the right. We keep doing this till n != 0 and incrementing count by 1 if we get a 1.

## Counting Bits:

- Brute force: go through every number in the range and do number of 1 bits on it.
- DP: When we reach a new power of 2, we add one bit. But, we already calculated the number of bits to the right of this new power of 2 (the leading bit). This is the number of bits present at dp[i - leading]. We go back by the new power of 2. Thus, our approach would be to initialize a dp array of size (n + 1). We set our leading bit to a 1. We then iterate from 1 to n + 1. If our leading bit * 2 == i (this means we reached a new power of two), we set our leading bit to i. Else, dp[i] = 1 + dp[i -leading]

## Reverse Number:

- We need to extract the right most bit and then set the left most of the result.
- We can extract the right most bit by doing n % 2. We then set the result through OR: result = result | (bit << 31 - i). This is because at the 0th position, we shift 31 times, at the first position, we shift 30 times, etc).
- We then get rid of the last bit through n = n // 2.

## Missing Number:

- Observation: sum of all the numbers in the range minus sum of al the numbers in the list will only leave out the number in the range that is not in the list.
- This allows us to find the result.
- Make sure to iterate through i, len(nums) + 1 since len(nums) is inclusive. Then have a check to add to the sum of the numbers in the list to not go out of index.
- Another observation: if we consider all the numbers and their indices, the only two numbers that appear uniquely is len(nums) and the missing numbers. We can include len(nums) in this by adding 0 (xoring 0 is just returning itself). Then, we get the unique number.

## Sum of Two Numbers:

- Adding two numbers can be done through XOR. Handling for carries can be done with AND.
- If we treat b as a carry, we are done when we don’t have a carry anymore. We can treat the problem as carrying over whatever value is in a over to b.
- Thus, we hold the carry in a temp as the (a & b) << 1 (an and at a specific place implies a carry over in the next column).
- We then set a to the naive sum (a ^ b) anded by the max possible integer in 32 bits
- We then set b to the carry by anding with the max possible integer.
- After this, if our result is exceeding the max possible integer, the way python works, we know its negative. If its not exceeding just go ahead and return the number at hand. However, if it is, then we need to worry about the 32 bottom most bits. We can flip them with ^ w/ min, and then making the leading 0s 1s in compliance w/ 2s compliment by negative. (thus ~(a ^ min)).

## Reverse Integer:

- Simplify by just using absolute value and using the positive.
- Keep a sign tracker.
- Essentially have a while loop for x. Get the last digit. Now, if the result so far is > to everything MAX except the last digit, adding anything would cause an overflow. Return 0. If it’s point for point equal, if the digit we just extracted would cause it to overflow, return 0.
- Then, set result to result * 10 + digit.
- If this result * sign is  outside the range mentioned, return True. Else return the result times the sign.

## Rotate Image:

- Notice that a transposition is just a flip across the diagonal and then reversal of reach row.
- Diagonal flip: for i in range(rows): for j in range(i, rows): m[i][j], m[j][i] = m[j][i], m[i][j].
- Then, use two pointers for each row to reverse.

## Spiral Matrix:

- We have four pointers. Two that are inclusive and two that are exclusive. Left and top are inclusive pointers starting at 0. Right and bottom are exclusive pointers (meaning they start at the ends of the matrix).
- We have a while loop that checks validity (left < right) and (top < bottom).  We then first get everything in the first row. Loop through [left, right), and add to the result m[top][i]. We then increment top. We then iterate on the right (right - 1). Iterate from top to bottom in similar fashion, and add to the result.
- We now do a check to see whether the condition is invalid because it is possible it is.
- We now need to iterate backwards. Thus, start from right - 1, left - 1, -1. And then iterate and add.
- Then we add what’s on the left, we start from bottom - 1, top - 1, and then add to result.
- We return result

## Set 0s:

- Brute force: iterate through the array and then find the 0s. When we find a 0, we record the row and column of it. We then iterate over the array again. If the row or column of it is in the appropriate set, we know that we have to zero that out. So we zero it.
- Optimal: we use the first row and first columns as markers. This creates overlap however, as the first entry would be both tied to a row or column. We thus create an additional boolean for the first row. Iterate over the matrix. If we get a 0. If its in the first row, set the boolean to true. Else, set m[r][0] to 0. We also set m[0][c] to 0 as well. Now, our first row and columns tell us what to zero out. We then iterate over the non-markers. If the [r][0] = 0 or [0][c] = 0, then set that to 0.
- We now need to check the first row and first column. Do similar operations for them.

## Happy Number:

- Brute force: Cycle detection question: have a helper function that calculates happiness. Have a visited set. While True, we calculate the happy version of this number, if its in the set, return false. If its equal to one, return True. else, set n to the happy number. Return false outside the while loop (even though we would never reach this point).
- Use Floyd’s Algorithm: we start off with slow pointer at n, fast pointer at happy(n). While true, we see whether fast is at 1 (this is to check for the trivial case initially). If it is return true. If slow and fast end up equalling, that means we found a loop. Else, move the slow pointer by one, else move fast pointer by two.

## Add One:

- For some reason I always trip up in this question. The idea is that if we iterate backwards over the digits and we find a digit that when we add to doesn’t go to 10, we can just add one to that digit and return the number. Else, we set that to 0. That way the next iteration, if we reach a number that doesn’t cause a sum to ten, we can just increment and return. To handle a chain such as “999”, our current process would just cause 0s for all three of them. When we reach 0 index, we have to do [1] + digits (where each of digits is 0).

## Pow(x, n):

- Brute force: keep multiplying a result by x n times. Result is initialized to 1. Then if n was negative, we return 1/result. Else we just return result.
- Optimal: if we have an even number we can split it up in x^(n//2) * x^(n//2). If its odd, we can do the same, but we just need to multiply x to itself. Notice how this is a recursive problem. To find each of the n//2, we can apply the same process. Our base case becomes when n == 0. We return 1. So what we can do is in our recursive function, we find the half. We then multiply half by its half. And if our exponent is odd, we have to multiply the two halves multiplied by x. Now, we do this recursive function on (x, abs(n)). If n < 0, return 1/result of the function. Else, just return the result

## Multiply Strings

- Just emulate the flow of multiplication. In our situation, we will make nums2 the larger number, and nums1 the smaller one. We have a result, and a factor. We then use a right pointer to get the rightmost value in the string (len(nums2) - 1). While this is greater than -1, we then extract that digit. We then will calculate the subsum of multiplying that digit with everything that is in nums 1. Thus, we have a subresult, a carry over (which is 0), and a power (set to 1). We then iterate in reverse over all the numbers in num1, we add to the subresult the current power * the modulo’d digit plus (carry * power). Carry is then set to the the floor division of 10. If we still have a carry after the loop add it to the subresult * power. We then add the subresult to the result * factor. We multiply the factor * 10. We decrement our pointer. At the very end we return str(result).

## Detect Squares:

- Brute force: store each point, and then calculate euclidean for sets of all possible three points with the target point. If they are all equal, it’s a valid setup.
- Optimal:
- We have a set to store unique points and a frequency map. When we add, we add to the set, and we also increase the frequency of the point.
- When we want to see all possible squares, we see whether each of the points can be considered a diagonal. If the x_delta and y_delta are equal, then for the other two points to be squares, the original x and target y, and target x and original y need to be considered.
- If these exist, then we can add to the count (frequency of diagonal * frequency of first other point and frequency of second other point).
- If the diagonal is the query itself, we continue.
- Return count

## Network Delay Time:

- Classic Djikstras: BFS with a heap.
- Create an adj list in [weight, edge] format. Then, create a heap that has [0, k] initially and a visited set. We then have a time variable. While heap, we pop node with the lowest weight so far. If the node in v, continue we seen it. We set t = max(weight of t), since weight is how much time it takes to reach that node. We then add node to visited. Then we all adjacent neighbors with the cost added to time. If discount neighbors based on visited, this could eliminate shorter paths. We keep doing this till the heap is empty. If our visited set is equal to n, return time, else we return -1 since we haven’t reached everything.

## Min Cost to Connect Points:

- Prim’s (create a minimum spanning tree): Create an adjacency list that is two directional. We use manhattan distance for both.  We create a heap with first point and distance 0. We have a visited set. While the len of the visited set is not equal to the total number of points, we pop from the heap to get the smallest distance so far. If we have seen it we continue. Else, we add to visited set and we add to our total cost. Then, we consider all shrotest paths from this point based on the adjacency list.
- Make sure the adjacency list created is bidirectional
- However, forming adjacency list is an O(n^2) operation.Can we make this solution more efficient?
- Essentially do a linear pass. We initialize n to len(points), node = 0. Distance is set to float(‘inf’) to every single point. Visited array is created with falses. We have a result and edges equal to 0. While edges < n - 1, we try to find the next best node to jump to. To figure this out, we iterate over every single node, get the distance from teh current node to it, and we minimize the distance to the node. If our nextNode is not set or the distance that we have is less than the next_node, we set next_node to this current one. Implement visited checks as well At the end of this loop, we should have the next node to go to from the node that is the shortest. We then move our node to their, and then we update the edges to be += 1, and add to our cost the distance to the nextNode.

## Cheapest Flights within K stops:

- Bellman Ford: use DP in order to find shortest path with additional constraints (such as when negative cycles).
- We initially create a DP array (the source is 0, and everything else is infinity). We can't reach there.
- We make a copy.
- Next, for k times, we go through all the flights. This simulates a stop since a stop would involve taking another flight. After the first pass, we simulate direct flights (essentially from source, can we directly go to the destination). After the second pass, we simulate one stop (two flights, one stop).
- For relaxation, we have the original distance array and then we add the cost to it. If this is less than the temp[dest], then we set it to that.
- We don’t use the temp for the start since this can change during the relaxation process. We only alter the temp’s values.
- We do this k + 1 times. If we did k, only would run it k - 1 times. Need to run this k times. Additionally if we want to run this with 0 stops, we need the loop to run once so k + 1.
- Then, see whether the destination is reachable. If destination is not reachable (still infinity), we can’t reach it within the number of stops so return -1. Else, return the value of the array at destination.
- 2 edges, 1 stop, 1 edge 0 stops. Since our loop runs the same number of times as the wanted number of edges, we want to use k + 1.

## Reconstruct Itinerary:

- Finding an Eulerian Path.
- What is an Eulerian Path: we traverse each edge once (can revisit vertices).
- Minimum Spanning Tree: we can visit each vertex once, Eulerian Path: we can visit each edge once.
- Because we need to do always pick the lexicographically smallest airport, we can sort the input airports such that we are sorting by the reverse of the last value.
- We then create the normal adjacency list.
- We do the following in our dfs:
- If our adjacency list is empty we add the value to the results then we return.
- Else, while the adjacency list is not empty (we consider all single outbound flights), we pop the destination flight. After we pop the destination flight, we run dfs on this.
- After, we would add this to our results. So a post order operation. Through this post order operation, we add all the flights that are after this one to the  results first, and this one. Thus, when we reverse the result in a topo sort type of fashion, we have a correct order.
- We then reverse the output.

## Swim in Rising Water:

- Modified Djikstras. Essentially, the “t” variable is impacted by what path we take. We want to take a path from top left to bottom right that has the smallest possible max value in that path. This is because that becomes our “t” and when we hit that t we can traverse the path.
- So we will store the maximum we have encountered at each cell. We always pick the smallest maximum. That way, when we reach the end right hand cell, we know we have the smallest possible way to get to this place.
- Heap, init to float(‘-inf’) first location. Pop from heap. If we see its in visited set continue. If its equal to the last possible position, max out the current max with the last cell and return that.
- Else, if its bounds, add. Update the path max.
- When trying to selectively get a path from A to B where you don’t know validity, always add all options.
- If you were solving longest increasing path or something like that, we know what makes path invalid. So, we can immediately not add things.

## Alien Dictionary:

- The question is focused on developing a graph. We need to verify that the the ordering provided is lexographically correct. In the case where it isn’t we would get a situation like “abc”, “ab”. String with all matches, the shorter string would be expected to come first. Thus this is incorrect.
- When creating our adjacency list, we would first create defaultdicts for every single character. This is because we only add to the defaultdict adjacency list in the case of a mismatch.
- After that, we iterate through each of the words two at a time (i, i + 1). We see the minimum length word. Now, if the first word is greater than the second one, and the two words have the same prefixes (minimum length word), we know that our condition has been violated. From there, we return “”. Else, we iterate over the two words, and we see the mismatch. The mismatch where the first one differs is the “smaller” character and will thus point to the “larger” character.
- From there, we do the classic visiting/visited set approach to do cycle detection. We then run this on every single character in the keys of the hashmap. If we ever get a false from our cycle detection we have an invalid configuration so return False. Else, return result reversed but joined as a string.

## Unique Paths:

- Recursion: do dfs(r, c). When we hit the n - 1 and m - 1, we know we reached the bottom right, so we return 1. That’s one possible way. We then recurse to dfs(r + 1, c)  and dfs(r, c+ 1). We then sum them and return the value. This is because the number of ways to get to the right corner from a certain point would be the sum of the ways to get to the right corner from valid states we can go to from here. We can easily memo this. No visited set because a block can be used in two paths to the same
- DP: common sense. We make a dp that (m * n). In this, we initialize the last item to 1 (since there is one way to reach the end). We then iterate backwards row wise and column wise and see the bounds of each r +1 and c + 1. If it’s within bounds we add it to the current dp entry.

## Longest Common Subsequence:

- Recursion: let’s think about the base cases. If we reach the end of one string, we return 0 because there is no commonality to consider. If the components of the string we are at are matching, we can pick this and then consider the longest common subsequence of the remainder string. If they don’t match, we can only pick one part of the string (both can’t appear in the common subsequence). Thus, we take the max of either. This obviously has repeating work, so can memo.
- DP: create DP table of len(t1) + 1 x len(t2) + 1. Needs to have one more to handle the case that we have reached the end of the string. Then, we fill backwards (iterate the strings in reverse) based on the recursive relationship mentioned (if equal, 1 + dp[i + 1][j + 1]), else max of pick one.

## Best Time to Buy or Sell Stock:

- Important to note that to reduce dimensionality of a DP problem, get something to propagate throughout calls. In this case it would be the actual profit.
- Recursively, we track of what index we are in, as well if we are allowed to be. If our index is the end of the price array or greater than, return 0.
- If we are allowed to buy in our current state, we can either buy or cool down. In the case of a buy, we get a -prices[i] + dfs(i + 1, not Buy). Cooldown, we just skip: dfs(i + 1, buy) (buy variable retains its value). In the case we are not allowed to buy, we can sell or do a cooldown and skip. If we sell, we get the prices[i] + dfs(i + 2, not Buy) (we skip an additional day and can directly buy that day). If we cooldown, we get dfs(i + 1, b). Return max of both.
- Can memo this due to repeated work.
- DP: need to make sure you are using calculated results and not using junk results. We have a matrix where columns is len(word) + 2 (since we do dp(i + 2) calls) and there are two rows. We iterate over the columns backwards first, and we iterate over the rows in the inside. If the row value is 1, we do the above logic but we make sure to flip the row. If the row value is 0, same logic as above. We use the idea that row value of 1 is False and row value of 0 is True.

## Coin Change II:

- Recursion: to get the total number of ways to get a particular target, we need to count ways of getting to the leaf of the recursion tree. If we are at the last index or our amount is negative, we haven’t gotten to the target so return 0. If our amount has hit 0, return 1. We then simulate picking a coin or not picking a coin.
- Picking a coin: dfs(i, amount - coins[i]) (since we can pick this coin again)
- Not picking a coin: dfs(i + 1, amount)  (skipping this).
- We return sum of these.
- We can memo due to repeated work.
- DP: we create a matrix where columns represent the array length + 1, and the rows are amount + 1 (to account for 0). Top most row is the amount = 0 situation (we fill this with ones). Then, we fill column-wise backwards, going sequentially down the rows. This is because thats how the DP table is filled. Our answer is thus stored in the first entry of the last row.

## Target Sum:

- Recursion: same idea as Coin Change. This time, we always advance index, but we can pick negative or positive. Be mindful of base cases when i == len(nums) and amount is equal to target we return 1. Have this check first. Then have the i >=  len(nums) invalid check, return 0. Then have both cases.
- DP: doesn’t make ssense to have a DP matrix in this case because we would to have 2 * target range because of the possibility of negatives. Use a defaultdict instead to only have values that make sense. Each index has a defauldict with a sum and frequency. We iterate over each possible index and then we get every sum and count pair. We then set the next row of the dp (dp[i + 1][sum + nums[i]] or dp[i + 1][sum - nums[i]]) to the frequency of the previous ones. Since the number of ways to get there is equal to the sum of the number of ways to get to the previous states.

## Interleaving Strings:

- Recursion (3 variables). Pretty straightfoward. We have a pointer for string 1, a pointer for string 2, and a pointer for string 3. If we reach the end of string 3 we see whether i == len(s1) and j == len(s2). If we exhaust one of the strings earlier, we want to see whether the other string matches up with the rest of the remaining big string. So if s1 exhausted, s2[j:] == s3[k:]. Vice versa. Then we do the actual logic checks. If neither position of either string matches the point we are in the third string, return False automatically. If both parts of the string matches, then we could use either one. So we want the or of the two because we just need one path to True. If only one matches (either left or right), do that particular one.
- Can break this down to only two variables. The way to do this is by realizing that the position in s3 we are is just the sum of s1 and s2. Additionally, we need to do a check whether the two sums actually equal the length of the third string. Creating the Dp array requires understanding what dimension is what string. Based off of that just draw it out and think about the dimensionality. Understand what is a row and what is columns. Dp table has columns of len(s1) + 1, has rows of len(s2) + 1. We fill the right most parts of the table based on what has been exhausted. Then we fill bottom up to the left. Outer loop is the rows (so string 2) the inner loop is columns (so string 1). Iterate backwards. Use same principles as above.

## Longest Increasing Path:

- Naive: DFS on the entire grid where we calculate max possible path from each cell. Don’t worry about a visited set because this is a DAG since it needs to be strictly increasing (no cycles). Check for invalidity (if r < 0, all that). If so, return 0 (path is not increasing there). Then have a increasing path set to 1 (we are counting the current cell). Then check validity of (r + 1, c), (r - 1, c), (r, c - 1), (r, c + 1) and see WHETHER ITS STRICTLY INCREASING COMPARED TO THE CURRENT. If so, max with the current max and 1 + the path (r + 1, c, or wtvr). Keep doing this, and then return it. Run this DFS on the entire grid.
- Dp (Top-Down): when we calculate the longest increasing path for a single cell, we calculate all possible paths from there that are valid. Thus that contains the longest increasing path for sure. We can memo this, and utilize this memo.

## Unique Subsequences:

- Recursive: we know we have a successful subsequence if j pointer reaches end of second string. Thus return 1 in that case. Any other situation where we are out of bounds, we return a 0. If our character at i and j match, we have the option of picking or not picking. We take the sum of those two (dfs(i + 1, j + 1) + dfs(i + 1, j).). Else if no match, we just take the no-match option (dfs (i +1, j), as the next character in the first could yield a match). Obviously can memo this.
- DP: be very careful about what dimension is what. Initialize 2d DP table with s being the columns and t being the rows. We then set the last row equal to 1. And the last column 0 to match the base cases. We then iterate backwards (columns inner, rows outer) and apply the logic above.
- WHAT MESSED ME UP: there is NO NEED To have a +1 in the recursive match when we take the sum of the two. We only have 1 when we reach the end, we don’t care about each character match.

## Edit Distance:

- Recursive: base case, when we have reached the end of the string for both, 0 edits needed for them to match. If we have reached the end of the first string, we need to delete the remainder of the last string in order for a match to occur (so len(second) - j). The similar principle applies the other way (reached the end of the second string, get rid of remainder of first string for a match). In the event two characters are equal, we move to onto the next. However, in the event two characters are not the same we have to apply the three operations and the pick of them.
- If the two characters are not the same, we can do a replacement and move both indices (i + 1, j + 1).
- If we delete whatever is at word1[i], we still have to check word1[i + 1] with the rest of the word2[j:], so dfs(i + 1, j)
- If when would we need to insert, we “pretend” to insert. We then know word2[j] has a match with word1[i]. thus , we check dfs(i, j + 1).
- Add 1 to both because we made a move.
- Can easily memo because repeated work
- Dp: 2d table. Be careful of columns and what they represent. Ensure the basecase is correct. The last column would be the length of the other string - other index.

## Burst Balloons:

- Want to arrive at a very efficient sub problem.
- Choose subarrays over subsequences in order to reduce state space size.
- If you pop something first, you create interdependency between the two arrays on the left and right.
- This is because each value dynamically changes depending on decisions that are made in another branch.
- If you pop something last, that means that corresponding subproblems are not dependent on each other.
- Thus, add [1] to left and right of the input array.
- Then create a dp array thats of length array (no need to pad here because the input array itself was padded). Then we define our dfs. Our dfs will try to calculate the maximum number of points attained within a particular interval in which we iterate through each coin within the interval and see what the number of points would be if we pop it last.
- Thus, if l > r, our interval is empty so return 0. If we saw dp[l][r] within our cache return that value. Then, for each i within [l, r], we calculate the value of popping this last (nums[i] * nums[l - 1] * nums[r + 1]). We then add the value of popping whole the subarrays dfs(l, i - 1) + dfs(i + 1, r). We then max out the dp value. Return the dp value.

## Regular Expression Matching:

- Two pointers, one for the string, one for the pattern. If we reach the end of both, we know we have a successful match. However, if we reach the end of the pattern but not the main string, our pattern didn’t fully match the main string, return False.
- We then see if there is match, there can be match if the two characters literally match or if there is a “.” match. We also need to check whether i index is within range for s.
- Afer this, we check whether there is a wildcard (*). Do so by checking if j + 1 is within bounds and if p[j + 1] is a “*”. If there is a wildcard, we can skip it entirely by saying that there is a match of 0 character (dfs(i, j + 2)). We can also extend the current match with this wildcard (m and dfs(i + 1, j)) only if what we had was a match. Elif we just have a match, we do a normal string match dfs(i + 1, j + 1). Else we return False.
- This can be memod because of repeated work. Just do top down bottom up can get really confusing.
