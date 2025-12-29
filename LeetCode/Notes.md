
# Prefix Sum
![[Pasted image 20251218163207.png]]
- Good for questions asking for the sum of subarrays
- Making an array where array[i] is the sum of all values until index i
- P[j] - P[i-1]
- Problems
	- When you make the prefix array make it len(n) +1 so its easier to do the P[j] - P[i-1] formula
	- Range Sum Query
		- This is just an easy question so just follow the general notes above
	- Contiguous Array
		- Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.
		- Make a prefix array except just consider the 0s as -1 and 1 as 1, so it acts like a counter
		- make a hash table to keep track of the last time you saw a value in the prefix array
		- Using that hash table you can easily find the longest by looking for equal values of prefix[i] in the hash table.
	- Subarray Sum Equals K
		- Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.
		- A subarray is a contiguous **non-empty** sequence of elements within an array.
		- make the usual prefix sum array, then youre basically looking at each index of prefix[i] how much you need to get to k
			- You can do this by making a hash table for those k - prefix[i] values. This hash table counts the number of times you see that value as you iterate, so just add this to your total count. 

# Two Pointers
- Use this when the problem is asking for pairs or elements in a sorted list that must satisfy a condition
	- Problems
		- Two Sum II
			- Given a **1-indexed** array of integers `numbers` that is already **_sorted in non-decreasing order_**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.
			- Return _the indices of the two numbers,_ `index1` _and_ `index2`_, **added by one** as an integer array_ `[index1, index2]` _of length 2._
			- The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.
			- Classic Two Pointers l,r at start and end, then just increment based on if total is less/greater than target
		- 3 Sum
			- Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
			- Notice that the solution set must not contain duplicate triplets.
			- Iterate through each element and use 2 sum to find the target which is just 0-nums[i]
				- To avoid duplicates when iterating just keep track of a prev and if its the same skip the 2 sum for that
				- Inside the 2 sum you should also do the same for the left pointer, if you've found a triplet of it already
		- Container With Most Water
			- You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.
			- Find two lines that together with the x-axis form a container, such that the container contains the most water.
			- Return _the maximum amount of water a container can store_.
			- This was lwk easier than it looked, just keep track of l,r (start and end) as usual, then get the area and compare each time
				- Increment l or r depending on which one has a smaller height.
# Sliding Window
- Used to find a subarray/substring that satisfies a condition
- Generally, it works by:
	- Updating r at the end of the loop
	- Checking if that new r broke the condition, if it did
		- probably update left until it's fixed again
		- You can typically track the condition using sets, flags or dicts
	- Problems
		- [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
			- Standard sliding window of moving l and r at the end, then comparing to prev averages
			- The optimization here is that averages can be compared with just sums, dont average (divide by k) until the return just compare sums
		- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
			- Use the set to track the repeating characters
			- while loop is while r < len(s)
				- if condition breaks and l < r
					- increment r  and remove from the tracking set
		- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
			- Similar to prev question except use a dictionary to track if anything is above 0 (negatives are ok)
# Fast and Slow Pointers
- This is used to identify IF a cycle exists, it doesn't find where the cycle is/where it starts
- You need to do the second half of the algorithm for it to find the cycle entrance:
```python
slow = nums[0]
        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]
```
-  Reset the slow to the start and increment both at the same speed
- Problems:
	- [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
		- This is just the first part of the algorithm to identify the existence of a cycle
			```python
			def hasCycle(self, head: Optional[ListNode]) -> bool:
			        slow, fast = head, head
			        while fast is not None and fast.next is not None:
			            slow = slow.next
			            fast= fast.next.next
			            if fast == slow:
			                return True
			        return False
			```

	- [Happy Number](https://leetcode.com/problems/happy-number/)
		- The important thing here is that you don't need a linked list, anything can act like a linked list for example here its the next function
		- Also here we don't need to find duplicates or anything so we don't need the second phase
	- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
		- Here we need to first use the first phase of the algo to enter the cycle (question already confirms that there is one)
		- Then use the second phase to just get the entrance
# Linked List Reversals
- you only need this for these types of questions where they tell you to do a reversal
- Generally you always want the prev, curr and some variant of the next pointer. 
	- Just remember that you shouldn't restrain yourself to this idea and that you should just imagine how each pointer is meant to change like in the third problem
- Problems
	- [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
		- Most basic / foundational algorithm solves this
		- prev will eventually end up in the last element of the linked list to reverse, which becomes the new head
	- [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
		- Just traverse until you reach the starting node of the part you want to reverse, then do the basic algorithm
		- Couple important changes / additions:
			- You are reversing a portion of the linked list (usually), so you need to save the node before and after that section you changed
				- Sometimes those can be nulls as the left or right could be the start or end of the original list
	- [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
		- This is where you shouldn't limit the basic algorithm as you're only way to solve these problems
			- The point of that algorithm was to teach you the logic of manipulating linked lists and saving important pointers
		- Important changes/ additions from previous questions:
			- You are basically reversing sets of linked lists (in this case sets of 2)
			- So in each iteration of the loop you are dealing with the 2 nodes, then in the next iteration you deal with the next 2
			- Thus, a couple important nodes you need to save are the starting node of the next set , and if the next set even has a second or first node to begin with (using the while condition `while curr and curr.next`)
				- I also made a second var, this is realistically just the nxt var i usually use but its definitely more intuitive more me
# Monotonic Stacks
- These are used when you want to find the next smallest/largest element of an element in an array. 
- The algorithm below is pretty universal
	- if you want the next biggest element, you want a decreasing stack
	- Just remember the time when you are popping is basically when you find that next smallest/largest element,  so this is your queue to do what you need to
```python
	class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        res = [-1] * len(nums1)
        nextLarger = {}

        # Use a monotonic decreasing stack
        for i in range(len(nums2)):
            while stack and nums2[stack[-1]] < nums2[i]:
                index = stack.pop()
                nextLarger[nums2[index]] = nums2[i]
            stack.append(i)
            if nums2[i] not in nextLarger:
                nextLarger[nums2[i]] = -1
        for i in range(len(nums1)):
            res[i] = nextLarger[nums1[i]]
        return res
```
- Problems
	- [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)
		- Just look at solution above
		- Something to note is that we just stored the values instead of indices in nextLarger (both key and value)
	- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
		- Only difference here is that instead of storing indexes, we just did a calculation for each index in the original array
	- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
		- This problem wants us to find the next smallest height, so when we pop we can keep track of how far left we can go
		- This problem has some confusing index calculation
		- Also when iterating, since we want to consider the entire width of the array, we want to make sure to go 1 over the length of the array to do that. 
# Top K Elements
- Finding the top k biggest/smallest elements 
- Just use heaps and sorting
	- In python use heapq.heapify(heap), heapq.heappush(heap, val), heapq.heappop(heap)
		- Note that when doing  heapq.heapify(heap), you **don't** need the heap =  heapq.heapify(heap). 
	- In python heapify is a min heap
		- Convert all to negative vals for max heap
- Problems
	- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- Literally just the basic algorithm of heapifying then popping k times
		- Make sure to convert to negatives for max heap, remember to convert res to positive again in the end
	- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
		- Remember you can do this for dictionaries in Python
			- `[[-v, k] for k,v in count.items()]`
		- when you heapify it sorts based on the first element if the heap is storing tuples/arrays
	- [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
		- the trick to this is just not adding all possible pairs, just push the first k rows, with just the first column, then pop in while loop, but after each pop, try adding the next column of the row you just popped, this automatically makes sure that if the next col is a duplicate value you also get that  
# Intervals
- Key here is looking for overlaps
	- An overlap occurs when your last interval's end > added interval's start, look for this and it's all good 
- Problems
	- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
		- Basically just check if end >= start and if it is just merge by changing the last interval's end to the max of both
		- else just append if not overlap
	- [Insert Interval](https://leetcode.com/problems/insert-interval/)
		- Best advice is to split cases between overlapping and non overlapping
			- Non Overlap is 2 cases
				- Before the current interval
					- If its before then just add the newInterval then the rest of the array
				- After current interval
					- If its after, then just insert the current interval (don't add the newInterval)
			- Overlap
				- combine the newInterval with the current one
				- `newInterval = [min(newInterval[0], inv[0]), max(newInterval[1], inv[1])]`
	- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
		- Here you don't need to make a merged list, just keep track of the end of the last node you omitted or kept (min of last node and node you might want to add)
		- Note this question is slightly different where overlap is only if the end > nextInterval's start
# Modified Binary Search

Problems
- In the while loop when you have l,r pointers, you typically want `while l <= r`
	- because you have arrays like `[1]` where l and r will be equal
- `mid = (l+r)//2`
- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
	- Determine which side of the array is sorted left side of mid or right side of mid
		- Check using `nums[l] <= nums[mid]` -> this means left side is sorted
		- with this you can check accordingly
```python
    def search(self, nums: List[int], target: int) -> int:
        l,r = 0, len(nums)-1
        while l <= r: #because of arrays like [1]
            mid = (l + r)//2
            if target == nums[mid]:
                return mid
            # Determine if left or right side of the array is sorted
            elif nums[l] <= nums[mid]:
                if target > nums[mid] or target < nums[l]:
                    l = mid + 1
                else:
                    r = mid-1
            else: # IF right side is sorted
                if target < nums[mid] or target > nums[r]:
                    r = mid -1
                else:
                    l = mid + 1
        return -1 
```
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
	- The condition is just if `nums[mid-1] > nums[mid]`
	- Then check `if nums[mid] >= nums[l]:`  
		- need >= because l = mid happens a lot, which is trivially true for sorted arrays
		- Means left side is sorted so just track the left most as the min for now
- [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
	- Not really a binary search
	- You just start at top right then go left column if target is smaller, go down a row if target is bigger
# Binary Tree Traversals
- PreOrder: root -> left -> right
- InOrder: left -> root -> right
- PostOrder: left -> right -> root
- Problems:
	- [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)
		- root to leaf would just be a preorder traversal
		- Every time you meet a node you add to path, then when there's a split you call the recursive traversal to account for the split
		- If you find a leaf just append it to the final result array
	```python 
				  def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
				        def preorder(node, path, result):
				            if not node:
				                return
				            path = path + "->" + str(node.val) if path else str(node.val)
				            if not node.left and not node.right:
				                result.append(path)
				            preorder(node.left, path, result)
				            preorder(node.right, path, result)
				        result = []
				        path = ""
				        preorder(root, path, result)
				        return result
	``` 
	- [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
		- Smallest is always bottom left, so inorder traversal
		- Global vars are very useful 
			- use nonlocal for them insider recursive function
	```python
	def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
	        # do a post order traversal
	        smallest = root.val
	        counter = k
	        def inorder(node):
	            nonlocal counter, smallest
	            if not node or counter == 0:
	                return
	            inorder(node.left)
	            counter -= 1
	            if counter == 0:
	                smallest = node.val
	                return
	            inorder(node.right)
	        inorder(root)
	        return smallest
	```
	- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
		- Use a global var again to keep track of the max
		- its ok to just add the curr val with left and right because we set the left and right to be at least 0, so negatives are accounted for already
	```python
	def maxPathSum(self, root: Optional[TreeNode]) -> int:
	        res = float("-inf")
	        def postorder(node):
	            if not node:
	                return 0
	            nonlocal res
	            left= max(0, postorder(node.left))
	            right=  max(0,postorder(node.right))
	            res = max(res, node.val + left+ right)
	            return node.val + max(left,right)
	        postorder(root)
	        return res
	```

# DFS
- Just used to traverse a graph to find a certain pattern
- Problems
	- [Clone Graph](https://leetcode.com/problems/clone-graph/)
		- use a hash map to map from old nodes to new nodes (copied nodes)
		- Whenever you call dfs(node)
			- you are creating the copy if its not made
			- if its made already you know you made its neighbors already
			- If not then you iterate on its neighbors and make copies for those
	```python
	def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
	        if not node:
	            return None
	        new = {}
	        def dfs(node):
	            if node in new:
	                return new[node]
	            copy = Node(node.val)
	            new[node] = copy
	            for n in node.neighbors:
	                newNei = dfs(n)
	                copy.neighbors.append(newNei)
	            return copy
	        return dfs(node)
	```
	 - [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
		 - just make a hashmap to make it easier for a dfs traversal
		 - need to make a set for detecting cycles
			 - When dfsing, check if its in that cycles set, if not add it, then start dfsing on the prereqs
				 - After that iteration is done remove it from the cycle set and add to output
			- With our output array we can use that to check if a course was done already, so we can early exit in dfs if we already took it
	```python
	def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
	        prereqs = {i:[] for i in range(numCourses)}
	        for c, p in prerequisites:
	            prereqs[c].append(p)
	        output = []
	        visit = set()
	        cycle = set()
	        def dfs(c):
	            if c in cycle:
	                return False
	            if c in output:
	                return True
	            cycle.add(c)
	            for pre in prereqs[c]:
	                if dfs(pre) == False:
	                    return False
	            cycle.remove(c)
	            # visit.add(c)
	            output.append(c)
	            return True
	        for i in range(numCourses):
	            if dfs(i) == False:
	                return []
	        return output  
	```
	- [Path Sum II](https://leetcode.com/problems/path-sum-ii/)
		- Just remember that in python when you pass a list, dict, and other objects into a function, it doesn't make a copy so u are modifying the same object again and again
	```python
	 def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
	        res = []
	        def dfs(node, total, path):
	            if not node: 
	                return 
	            total = total + node.val
	            path = path.copy()
	            path.append(node.val)
	            if not node.left and not node.right and total == targetSum:
	                res.append(path)
	            dfs(node.left, total, path)
	            dfs(node.right, total, path)
	        dfs(root, 0 ,[])
	        return res
	```

# BFS
- Use a deque and sometimes a visited set
- You usually need to account for empty inputs since the while loop usually won't deal with it
- When you append the q with the new elements, this is typically when you want to add to the visited too.
- In python you need to import the deque
- Problems
	- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
		- Very Standard BFS
	```python
	from collections import deque
	class Solution:
	    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
	        if root == None:
	            return []
	        q = deque()
	        q.append(root)
	        output = []
	        while q:
	            length = len(q)
	            sub = []
	            for i in range(length):
	                popped = q.popleft()
	                if popped.left:
	                    q.append(popped.left)
	                if popped.right:
	                    q.append(popped.right)
	                sub.append(popped.val)
	            output.append(sub)
	        return output
	```
	- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
		- The difference here is the way we get new things in the deque
		- We also only add fresh oranges to explore, since we already explored rotten oranges
	```python
	from collections import deque
	class Solution:
	    def orangesRotting(self, grid: List[List[int]]) -> int:
	        # Do a bfs
	        # put rottens in the deque at first 
	        # keep a dq of (r,c)s
	        # keep track of time
	        # keep track of number of fresh oranges currently and prev
	        # if number of fresh oranges not == 0 when cur = prev then its impossible
	        q = deque()
	        curFresh = 0
	        prevFresh = 0
	        for r in range(len(grid)):
	            for c in range(len(grid[0])):
	                if (grid[r][c] == 2):
	                    q.append((r,c))
	
	                elif grid[r][c] == 1:
	                    curFresh += 1
	        prevFresh = curFresh
	        time = 0 
	        visited = set()
	        directions = [[-1,0], [1,0], [0,-1], [0,1]]
	        while q and curFresh>0 :
	
	            length = len(q)
	            for i in range(length):
	                popped = q.popleft()
	                
	                for dr, dc in directions:
	                    if popped[0] + dr in range(len(grid)) and popped[1] + dc in range(len(grid[0])):
	                        new = (popped[0] + dr, popped[1] + dc)
	                        if new not in visited and grid[popped[0] + dr][popped[1] + dc] ==1:
	                            q.append(new)
	                            visited.add(popped)
	                            if grid[popped[0] + dr][popped[1] + dc] == 1:
	                                grid[popped[0] + dr][popped[1] + dc] = 2
	                                curFresh -=1
	            time += 1
	        return time if curFresh == 0 else -1
	```

# Matrix Traversal
- Utilize DFS or BFS to search from a particular point like looking for islands
- Problems
	- [Flood Fill](https://leetcode.com/problems/flood-fill/)
		- Standard BFS works well here
		- Just remember the first/starting point needs to be changed too (i did this before the while loop)
	```python
	from collections import deque
	class Solution:
	    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
	        visited = set()
	        visited.add((sr,sc))
	        q = deque()
	        q.append((sr,sc))
	        original = image[sr][sc]
	        image[sr][sc] = color
	        directions = [(1,0), (-1,0), (0,1), (0,-1)]
	        while q:
	            popped = q.pop()
	            for dr, dc in directions:
	                nr = dr + popped[0]
	                nc = dc + popped[1]
	                if nr in range(len(image)) and nc in range(len(image[0])) and image[nr][nc] == original and (nr,nc) not in visited :
	                    visited.add((nr,nc))
	                    q.append((nr,nc))
	                    image[nr][nc] = color
	        return image
	```
	- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
		- Also can use BFS
		- Basically the same thing as last question except make bfs a helper, and run on any square that is a "1", to convert all adjacent ones to "0"
			- Every BFS run, you increase the counter
	```python
	from collections import deque
	class Solution:
	    def numIslands(self, grid: List[List[str]]) -> int:
	        def bfs(r,c):
	            visited = set()
	            q= deque()
	            q.append((r,c))
	            visited.add((r,c))
	            grid[r][c] = "0"
	            directions = [(1,0), (-1,0), (0,1), (0,-1)]
	            while q:
	                popped = q.pop()
	                for dr,dc in directions:
	                    nr = popped[0] + dr
	                    nc = popped[1] + dc
	                    if nr in range(len(grid)) and nc in range(len(grid[0])) and grid[nr][nc] == "1" and (nr,nc) not in visited:
	                        visited.add((nr,nc))
	                        q.append((nr,nc))
	                        grid[nr][nc] = "0"
	        count = 0
	        for r in range(len(grid)):
	            for c in range(len(grid[0])):
	                if grid[r][c] == "1":
	                    bfs(r,c)
	                    count += 1
	        return count
	```
	- [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
		- This is very similar to previous question
		- You just run bfs on the borders and if its land
		- The difference is that you mark these lands connected to the border as some other character first and reconvert them later
	 ```python
	class Solution:
	    def solve(self, board: List[List[str]]) -> None:
	        """
	        Do not return anything, modify board in-place instead.
	        """
	        def bfs(r,c):
	            visited = set()
	            q= deque()
	            q.append((r,c))
	            visited.add((r,c))
	            directions = [(1,0), (-1,0), (0,1), (0,-1)]
	            board[r][c] = "T"
	            while q:
	                popped = q.pop()
	                for dr,dc in directions:
	                    nr = popped[0] + dr
	                    nc = popped[1] + dc
	                    if nr in range(len(board)) and nc in range(len(board[0])) and board[nr][nc] == "O" and (nr,nc) not in visited:
	                        visited.add((nr,nc))
	                        q.append((nr,nc))
	                        board[nr][nc] = "T"
	        for r in range(len(board)):
	            for c in range(len(board[0])):
	                if (r in [0, len(board)-1] or c in [0, len(board[0])-1]) and board[r][c] == "O":
	                    bfs(r,c)
	        for r in range(len(board)):
	            for c in range(len(board[0])):
	                if board[r][c] == "O":
	                    board[r][c] = "X"
	        for r in range(len(board)):
	            for c in range(len(board[0])):
	                if board[r][c] == "T":
	                    board[r][c] = "O"
	```

# Back Tracking
- Good for problems that need you to find all possible solutions (like permutations) or most solutions given a constraint
	- Usually these are the same except you just make a check before you call the helper backtrack function
- In backtracking you are going to share the lists you use to make the solutions, so you usually want to make copies before inserting into your output
- Remember in back tracking you need to do clean ups after the helper function call so it can explore more cases
- Problems:
	- [Permutations](https://leetcode.com/problems/permutations/)
		- Use permute to find the permutations when you exclude the first element
		- Then insert, the element into every possible position
	```python
	class Solution:
	    def permute(self, nums: List[int]) -> List[List[int]]:
	        if len(nums) == 0:
	            return [[]]
	        perms = self.permute(nums[1:])
	        res = []
	        for perm in perms:
	            for i in range(len(perm)+1):
	                p_copy = perm.copy()
	                p_copy.insert(i, nums[0])
	                res.append(p_copy)
	        return res
	```
	- [Subsets](https://leetcode.com/problems/subsets/)
		- Use the helper to iterate each index and decide whether to include that index or not
		- Good example of cleaning up 
	```python
	class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        if len(nums) == 0:
            return [[]]
        res = []
        subset = []
        def helper(i):
            if i == len(nums):
                res.append(subset.copy())
                return
            
            subset.append(nums[i])
            helper(i+1)
            subset.pop()
            helper(i+1)
        helper(0)
        return res
	```
	- [N-Queens](https://leetcode.com/problems/n-queens/)
		- Put one queen in each row, check ifs ok to put there based on the column, pos and neg diags. 
			- If you can reach the end add to output
		- Not a scary hard, you just need to know how to check conditions for diagonals 
			- positive diagonals = row + column index is constant across a diagonal
			- negative diagonals = row - column
		- Also do clean ups after calling the helper function
	``` python 
	class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        cols = set()
        posDiags = set()
        negDiags = set()
        res  = []
        board = [["."]* n for i in range(n)]
        def backtrack(r):
            if r == n:
                copy = board.copy()
                copy = ["".join(row) for row in board]
                res.append(copy)``
            for c in range(n):
                if c in cols or (r+c) in posDiags or (r-c) in negDiags:
                    continue
                cols.add(c)
                posDiags.add(r+c)
                negDiags.add(r-c)
                board[r][c] = "Q"
                backtrack(r+1)
                cols.remove(c)
                posDiags.remove(r+c)
                negDiags.remove(r-c)
                board[r][c] = "."
        backtrack(0)
        return res
	```
