
# Python making Lists tips:
- When you want to make a list like `list[x][y]` , the way you form it is like this
	- `list = [[startVal]* y for _ in range(x)]`
	- Basically you flip the order of `[x][y]` when making it
	- Its important to do `for _ in range(x)` when you are making copies of rows, because if you do `*x` instead, it will make copies to the exact same row
# 1. Prefix Sum
![[Pasted image 20251218163207.png]]
- Good for questions asking for the sum of subarrays
- Making an array where array[i] is the sum of all values until index i
- P[j] - P[i-1]
- Problems
	- When you make the prefix array make it len(n) +1 so its easier to do the `P[j] - P[i-1]` formula
	- [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)
		- This is just an easy question so just follow the general notes above
	```python
	class NumArray:
	    def __init__(self, nums: List[int]):
	        self.nums = nums
	        self.prefix = [0] *(len(nums)+1)
	        for i in range(len(self.prefix)-1):
	                self.prefix[i+1] = self.prefix[i] + self.nums[i]
	    def sumRange(self, left: int, right: int) -> int:
	        return self.prefix[right+1] - self.prefix[left]
	```
	- [Contiguous Array](https://leetcode.com/problems/contiguous-array/)
		- Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.
		- Make a prefix array except just consider the 0s as -1 and 1 as 1, so it acts like a counter
		- make a hash table to keep track of the last time you saw a value in the prefix array
		- Using that hash table you can easily find the longest by looking for equal values of prefix[i] in the hash table.
	```python 
	class Solution:
    def findMaxLength(self, nums: List[int]) -> int:
        #make an array len(nums) 
        prefix = [0] * (len(nums) +1)
        last = {}
        for i in range(len(nums)):
            if nums[i] == 0:
                prefix[i+1] = prefix[i] -1
            else:
                prefix[i+1]= prefix[i] + 1
        for i in range(len(nums)+1):
            last[prefix[i]] = i
        # make a dict where i track the last location of a num
        # iterate one time through prefix and calculate max distance using dict
        maxi = 0
        for i in range(len(prefix)):
            if maxi < last[prefix[i]] - i:
                maxi = last[prefix[i]] - i
        return maxi
	```
	- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
		- Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.
		- A subarray is a contiguous **non-empty** sequence of elements within an array.
		- make the usual prefix sum array, then you're basically looking at each index of prefix[i] how much you need to get to k
			- You can do this by making a hash table for those k - prefix[i] values. This hash table counts the number of times you see that value as you iterate, so just add this to your total count.
	```python
	class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        prefix = [0] * (len(nums) +1)
        for i in range(len(nums)):
            prefix[i+1] = prefix[i] + nums[i]
        # now we havethe prefix sum array
        # using this ?
        count = 0
        seen = {}
        seen[0] = 1
        for i in range(1, len(prefix)):
            current = prefix[i]
            need = current - k
            if need in seen:
                count += seen[need]
            if current not in seen:
                seen[current] = 1
            else:
                seen[current] +=1
        return count
	```

# 2. Two Pointers
- Use this when the problem is asking for pairs or elements in a sorted list that must satisfy a condition
	- Problems
		- Two Sum II
			- Classic Two Pointers l,r at start and end, then just increment based on if total is less/greater than target
		```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l,r = 0, len(numbers)-1
        while l < r:
            left = numbers[l]
            right = numbers[r]
            total = left + right
            if (total == target):
                return [l+1, r+1]
            elif(total > target):
                r = r -1
            else: 
                l = l + 1
        return [-1,-1] 
		```
		- 3 Sum
			- Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
			- Notice that the solution set must not contain duplicate triplets.
			- Iterate through each element and use 2 sum to find the target which is just 0-nums[i]
				- To avoid duplicates when iterating just keep track of a prev and if its the same skip the 2 sum for that
				- Inside the 2 sum you should also do the same for the left pointer, if you've found a triplet of it already
		```python
		class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # Iterate through each index, then do a 2 pointer 2 sum from that point 
        prev = -123123123
        res = []
        nums.sort()
        for i in range(len(nums)):
            if prev == nums[i]:
                continue
            prev = nums[i]
            l,r  = i+1, len(nums)-1
            target = 0-nums[i]
            while l < r:
                total = nums[l] + nums[r]
                if total == target:
                    res.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
                elif total > target:
                    r = r-1
                else:
                    l = l+1
        return res        
		```
		- Container With Most Water
			- Note that the array is not sorted, but that's ok
			- This was lwk easier than it looked, just keep track of l,r (start and end) as usual, then get the area and compare each time
				- Increment l or r depending on which one has a smaller height.
			```python
			class Solution:
			    def maxArea(self, height: List[int]) -> int:
			        maxArea = 0
			        l,r = 0, len(height)-1
			        while l < r:
			            w= r - l
			            h = min(height[l],height[r])
			            area = w*h
			            maxArea = max(area, maxArea)
			            if height[l] < height[r]:
			                l += 1
			            else: 
			                r-=1
			        return maxArea
			```
# 3. Sliding Window
- Used to find a subarray/substring that satisfies a condition
- Generally, it works by:
	- Start both l,r at 0, then update the thing you're checking at the start of the loop
	-  Checking if that new r broke the condition, if it did
		- probably update left until it's fixed again
		- You can typically track the condition using sets, flags or dicts
	- Once you fix it or not, check if you can update your output
	- Updating r at the end of the loop
	- Problems
		- [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
			- Question asks for max subarray length k
			- Standard sliding window of moving l and r at the end, then comparing to prev averages
			- The optimization here is that averages can be compared with just sums, dont average (divide by k) until the return just compare sums
		``` python
		class Solution:
		    def findMaxAverage(self, nums: List[int], k: int) -> float:
		        l,r = 0,0
		        total = 0
		        hi = float('-inf')
		        while r < len(nums):
		            total += nums[r]
		            if r-l+1 > k: 
		                total -= nums[l]
		                l += 1
		            if (r-l+1) == k:
		                 hi = max(hi, total)
		            r+= 1
		        return hi/k
		```
		- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
			- Use the set to track the repeating characters
			- while loop is while r < len(s)
				- if condition breaks and l < r
					- increment r  and remove from the tracking set
		```python
		class Solution:
		    def lengthOfLongestSubstring(self, s: str) -> int:
		        tracker = set()
		        # tracker to track count of chars
		        # iterate and have a maxcounter, then a sliding window approach of adding the next r and removing the previous l
		        l,r = 0,0
		        longest = 0
		        while r < len(s):
		            while l < r and s[r] in tracker: # check if this new r is in tracker, if it is keep removing l until
		            # its not
		                tracker.remove(s[l])
		                l+= 1
		            tracker.add(s[r]) # add the new r, because previously we incremented at the end of the loop
		            longest = max(longest , (r-l+1))
		            r+= 1
		        return longest
		```
		- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
			- Similar to prev question except use a dictionary to track if anything is above 0 (negatives are ok)
	``` python
	class Solution:
    def minWindow(self, s: str, t: str) -> str:
        # tracker hashmap to count the chars
        # chars to fix variable
        # minimum length right now
        # do a sliding window where:
            #move right at the end of the loop
            # move left when we fulfilled the condition, until its broken

        tracker = {}
        for c in t:
            tracker[c] = tracker.setdefault(c, 0) +1
        fixesLeft = len(t)
        minLen = 99999999999999999999999999
        minStr = ""

        def helper(tracker):
            for k in tracker:
                if tracker[k] > 0:
                    return False
            return True
        l, r = 0,0
        while r < len(s):
            if s[r] in tracker:
                tracker[s[r]] -=1
            while l <= r and helper(tracker):
                if minLen >  r-l+1 :
                    minLen = r-l+1
                    minStr = s[l:r+1]
                if s[l] in tracker:
                    tracker[s[l]] += 1
                l += 1
            r+= 1
            
        return minStr
	```
# 4. Fast and Slow Pointers
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
	```python
	class Solution:
    def isHappy(self, n: int) -> bool:
        def next(n):
            total =0
            while n > 0:
                d=  n % 10
                d = d*d
                total += d
                n = n//10
            return total
        slow = n
        if next(n) == 1 or next(next(n)) == 1:
            return True
        fast = next(next(n))

        while fast != slow and fast != 1:
            slow = next(slow)
            fast = next(next(fast))
            if fast == slow :
                return False
        return fast == 1
	```
	- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)
		- Here we need to first use the first phase of the algo to enter the cycle (question already confirms that there is one)
		- Then use the second phase to just get the entrance
	```python
	class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        slow = nums[0]
        fast = nums[0]

        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break
        slow = nums[0]
        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]

        return slow
	```
# 5. Linked List Reversals
- you only need this for these types of questions where they tell you to do a reversal
- Generally you always want the prev, curr and some variant of the next pointer. 
	- Just remember that you shouldn't restrain yourself to this idea and that you should just imagine how each pointer is meant to change like in the third problem
- If it helps having a dummy node is useful for the return, but if you're reversing a part of the linked list, just think of the node before the first reversed node as that dummy node.
- Problems
	- [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
		- Most basic / foundational algorithm solves this
		- prev will eventually end up in the last element of the linked list to reverse, which becomes the new head
	```python
	class Solution:
	    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
	        prev = None
	        curr = head
	        while curr:
	            nxt = curr.next
	            curr.next = prev
	            prev= curr
	            curr = nxt
	        return prev
	```
	- [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
		- Just traverse until you reach the starting node of the part you want to reverse, then do the basic algorithm
		- Couple important changes / additions:
			- You are reversing a portion of the linked list (usually), so you need to save the node before and after that section you changed
				- Sometimes those can be nulls as the left or right could be the start or end of the original list
	```python
	class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        if not head or not head.next or left == right:
            return head
        curr = head
        prev= None
        nxt = None
        for i in range(left-1):
            prev= curr
            curr = curr.next
        start = prev # Node before the first node to be reved
        leftStart = curr # first node to be reved (should be last in the end)
        for i in range (right-left+1):
            nxt = curr.next
            curr.next = prev
            prev= curr
            curr = nxt
        last = curr #node after the portion we reversed
        if start:
            start.next = prev
        if leftStart:
            leftStart.next = last

        return head if left != 1 else prev
	```
	- [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
		- This is where you shouldn't limit the basic algorithm as you're only way to solve these problems
			- The point of that algorithm was to teach you the logic of manipulating linked lists and saving important pointers
		- Important changes/ additions from previous questions:
			- You are basically reversing sets of linked lists (in this case sets of 2)
			- So in each iteration of the loop you are dealing with the 2 nodes, then in the next iteration you deal with the next 2
			- Thus, a couple important nodes you need to save are the starting node of the next set, the ending node of current set, and if the next set even has a second or first node to begin with (using the while condition `while curr and curr.next`)
				- I also made a second var, this is realistically just the nxt var i usually use but its definitely more intuitive more me
	```python
	class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = ListNode(0, head)
        start = prev
        curr = head
        nxt = None
        while curr and curr.next:
            nextSet = curr.next.next
            second = curr.next

            prev.next = second
            second.next = curr
            curr.next = nextSet
            prev = curr
            curr = nextSet
        return start.next
	```
# 6. Monotonic Stacks
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
	```python
	class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0] * len(temperatures)
        stack = []
        #need a decreasing monotonic stack
        for i in range(len(temperatures)):
            while stack and temperatures[stack[-1]] < temperatures[i]:
                index = stack.pop()
                answer[index] = i - index
            stack.append(i)
        return answer 
	```
	- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
		- This problem wants us to find the next smallest height, so when we pop we can keep track of how far left we can go
		- This problem has some confusing index calculation
		- Also when iterating, since we want to consider the entire width of the array, we want to make sure to go 1 over the length of the array to do that. 
		- To find its left boundary, we look at the element that is _now_ at the top of the stack (`stack[-1]`). This element `stack[-1]` represents the index of the **first bar to the left of `index` that is shorter than `heights[index]`**.
	```python
	class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []
        maxArea = 0

        for i in range(len(heights)+1):
            checkHeight= heights[i] if i < len(heights) else 0
            while stack and heights[stack[-1]] > checkHeight:
                index = stack.pop()
                height = heights[index]
                left= stack[-1] if stack else -1 # its -1 because left is exclusive 
                # left + 1 is the index with at least height
                width = (i -1)- left # no need +1 because left is exclusive
                maxArea = max(maxArea, width* height)
                
            stack.append(i)

        return maxArea

	```
# 7. Top K Elements
- Finding the top k biggest/smallest elements 
- Just use heaps and sorting
	- In python use `heapq.heapify(heap), heapq.heappush(heap, val), heapq.heappop(heap)`
		- Note that when doing  `heapq.heapify(heap)`, you **don't** need the `heap =  heapq.heapify(heap)`. 
	- In python `heapify` is a min heap
		- Convert all to negative values for max heap
- Problems
	- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- Literally just the basic algorithm of heapifying then popping k times
		- Make sure to convert to negatives for max heap, remember to convert res to positive again in the end
		```python
		class Solution:
		    def findKthLargest(self, nums: List[int], k: int) -> int:
		        heap = [-1*num for num in nums]
		        heapq.heapify(heap)
		        res = 0
		        while k >0:
		            res = heapq.heappop(heap)
		            k-=1
		        return -res
		```
	- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)
		- Remember you can do this for dictionaries in Python
			- `[[-v, k] for k,v in count.items()]`
		- when you heapify it sorts based on the first element if the heap is storing tuples/arrays
	```python 
	class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        for num in nums:
            count[num] = count.get(num, 0) + 1
        heap = [[-v, k] for k,v in count.items()]
        heapq.heapify(heap)
        res = []
        while k > 0:
            _, key = heapq.heappop(heap)
            res.append(key)
            k -=1
        return res
	```
	- [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
		- the trick to this is just not adding all possible pairs, just push the first k rows, with just the first column, then pop in while loop, but after each pop, try adding the next column of the row you just popped, this automatically makes sure that if the next col is a duplicate value you also get that  
	```python 
	class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        heap = []
        n1 = len(nums1)
        n2 = len(nums2)
        res = []
        for i in range(min(k, n1)):
            heapq.heappush(heap, [nums1[i] + nums2[0], i, 0])
        while k > 0:
            _, i, j = heapq.heappop(heap)
            if j+1 < n2:
                heapq.heappush(heap, [nums1[i] + nums2[j+1], i, j+1])
            res.append([nums1[i], nums2[j]])
            k-=1
        return res

	```
# 8. Intervals
- Key here is looking for overlaps
	- An overlap occurs when your last interval's end > added interval's start, look for this and it's all good 
- Problems
	- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)
		- Basically just check if end >= start and if it is just merge by changing the last interval's end to the max of both
		- Also note that sorting is needed
		- else just append if not overlap
	```python 
	class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort()
        merged = []
        
        for interval in intervals:
            if not merged:
                merged.append(interval)
                continue
            if merged[-1][1] >= interval[0]:
                merged[-1][1] = max(interval[1], merged[-1][1])
            else:
                merged.append(interval)
        return merged

	```
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
	```python
	class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        merged = []
        i = 0 
        for inv in intervals:
            # check overlapping vs non overlapping
            #-- Non Overlapping --
            # Before current inv
            if inv[0] > newInterval[1]:
                merged.append(newInterval)
                return merged + intervals[i:]
            # After current inv
            elif inv[1] < newInterval[0]:
                merged.append(inv)

            # -- Overlap --
            else:
                newInterval = [min(newInterval[0], inv[0]), max(newInterval[1], inv[1])]
            i += 1
        merged.append(newInterval)
        return merged

	```
	- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
		- Here you don't need to make a merged list, just keep track of the end of the last node you omitted or kept (min of last node and node you might want to add)
		- Note this question is slightly different where overlap is only if the end > nextInterval's start
	```python
	class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # Sort the intervals
        intervals.sort()
        # Iterate
        isFirst = True
        removed = 0
        for inv in intervals:
            # if start of traversal
            if isFirst:
                end = inv[1]
                isFirst = False
                continue
            # If Overlapping
            if end > inv[0]:
                end = min(end, inv[1])
                removed += 1 
            # Not Overlapping
            else:
                end = inv[1]
        return removed
	```
	- [Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)
		- Sorting is always good for intervals
		- use minheap to find the smallest size, but make sure to store each intervals' ending time to know when to pop / if something isn't usable anymore
		```python
		class Solution:
		    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
		        intervals.sort()
		        res = {}
		        minheap = []
		        i = 0
		        for q in sorted(queries):
		            while i < len(intervals) and intervals[i][0] <= q:
		                l = intervals[i][0]
		                r = intervals[i][1]
		                heapq.heappush(minheap, (r-l+1, r))
		                i += 1
		            while minheap and minheap[0][1] < q:
		                heapq.heappop(minheap)
		            res[q] = minheap[0][0] if minheap else -1
		        final = [res[q] for q in queries]
		        return final
		```
# 9. Modified Binary Search

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
	```python
	class Solution:
    def findMin(self, nums: List[int]) -> int:
        l,r = 0, len(nums)-1
        mini = float(inf)
        while l <= r :
            mid = (l+r)//2
            if nums[mid-1] > nums[mid]:
                return nums[mid]
            if nums[mid] >= nums[l]:
                mini = min(mini,nums[l])
                l = mid + 1
            else:
                mini = min(mini, nums[mid])
                r = mid-1
        return mini 
	```
- [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
	- Not really a binary search
	- You just start at top right then go left column if target is smaller, go down a row if target is bigger
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        #search between the rows  by looking at the max value (right most side)
        rows = len(matrix)
        cols = len(matrix[0])

        r = 0
        c = cols-1
        while r < rows and c >= 0:
            if matrix[r][c] == target:
                return True
            if matrix[r][c] > target:
                c -= 1
            else:
                r+= 1
        return False

```
# 10. Binary Tree Traversals
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

# 11. DFS
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
		 - just make a hash map to make it easier for a dfs traversal
		 - This is a topological sort question
			 - To do topological sort you must make sure its on a DAG (Directed Acyclic Graph)
				 - If not guaranteed, then you must keep track of a visited, cycle set
			 - Do dfs in a post order way, so append to output at the end so the order is correct
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
	                res.append(path) # if you do copy here instead of above, the path will get fucked if u don't reach targetSum
	            dfs(node.left, total, path)
	            dfs(node.right, total, path)
	        dfs(root, 0 ,[])
	        return res
	```
	- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
		- Using DFS you just need to keep track of the lower and upper bounds at the current node
			- so if you go left child update the upper bound, keep lower
			- if go right child, update lower bound, keep upper
		```python 
		class Solution:
	    def isValidBST(self, root: Optional[TreeNode]) -> bool:
	        def dfs(node, lower_bound, upper_bound):
	            if not node:
	                return True
	            if not (lower_bound < node.val < upper_bound):
	                return False
	            return dfs(node.left, lower_bound, node.val) and dfs(node.right, node.val, upper_bound)
	        return dfs(root, float("-inf"), float("inf"))
		```
	- [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
		- Just remember how to calculate tree height
	```python
	class Solution:
	    def isBalanced(self, root: Optional[TreeNode]) -> bool:
	        def dfs(node):
	            if not node:
	                return 0
	            left = dfs(node.left)
	            if left == -1:
	                return -1
	            right= dfs(node.right)
	            if right == -1:
	                return -1
	            if abs(left-right) >1:
	                return -1
	            return 1+max(left, right)
	        return dfs(root) != -1
	    def getHeight(self, node):
		    if not node:
			    return 0 
		    leftHeight = getHeight(node.left)
		    rightHeight = getHeight(node.right)
		    return 1 + max(leftHeight,rightHeight)
	```
	- ## Using Tries
		- [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
			- When you make a data structure to store and find words tries are the best
			- Make a TrieNode store the children and isWord value
				- Add is just make new node if child is not there
				- Search is use dfs/recursion if the current letter is "." (any word), else just do normal check if that letter exists as a child
			```python
			class TrieNode:
			    def __init__(self):
			        self.children = {}
			        self.isWord = False
			class WordDictionary:
			
			    def __init__(self):
			        self.root = TrieNode()
			        
			
			    def addWord(self, word: str) -> None:
			        curr = self.root
			        for c in word:
			            if c not in curr.children:
			                curr.children[c] = TrieNode()
			            curr = curr.children[c]
			        curr.isWord = True
			
			    def search(self, word: str) -> bool:
			        def dfs(j, root):
			            curr = root
			            for i in range(j, len(word)):
			                c = word[i]
			                if c == ".":
			                    for child in curr.children.values():
			                        if dfs(i+1, child):
			                            return True
			                    return False
			                else:
			                    if c not in curr.children:
			                        return False
			                    curr = curr.children[c]
			            return curr.isWord
			        return dfs(0, self.root)
			```
		- [Word Search II](https://leetcode.com/problems/word-search-ii/)
			- When keeping track of words, its very useful and efficient to use tries
			- Make a trie of the words you want to find and then dfs + backtracking to iterate over each starting position (r,c)
			- Remember backtracking you just need to go all directions, then remove the position from the visited list or whatever you used to keep track of progress
			```python
			class TrieNode:
			    def __init__(self): 
			        self.children = {}
			        self.isWord = False
			    def addWord(self, word):
			        curr = self
			        for c in word:
			            if c not in curr.children:
			                curr.children[c] = TrieNode()
			            curr = curr.children[c]
			        curr.isWord = True
			
			class Solution:
			    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
			        Trie = TrieNode()
			        for word in words:
			            Trie.addWord(word)
			        ROWS, COLS = len(board), len(board[0])
			        res = set()
			        visited = set()
			        def dfs(r,c, node, word):
			            if r < 0 or c < 0 or r >= ROWS or c >= COLS or board[r][c] not in node.children or (r,c) in visited:
			                return
			            visited.add((r,c))
			            node = node.children[board[r][c]]
			            word += board[r][c]
			            if node.isWord:
			                res.add(word)
			            dfs(r+1 , c , node, word)
			            dfs(r-1 , c , node, word)
			            dfs(r , c+1 , node, word)
			            dfs(r , c-1 , node, word)
			            visited.remove((r,c))
			        for r in range(ROWS):
			            for c in range(COLS):
			                dfs(r,c,Trie, "")
			        return list(res)
			```

# 12. BFS
- Use a deque and sometimes a visited set
- You usually need to account for empty inputs since the while loop usually won't deal with it
- When you append the q with the new elements, this is typically when you want to add to the visited too.
- In python you need to import the deque
- use `popleft()` for bfs
- Problems
	- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
		- Very Standard BFS
	```python
	from collections import deque
	class Solution:
	    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
	        if root == None: # you will usually need this
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
	- [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)
		- Use BFS to explore from leaf nodes (layer by layer)
		- The property here is that you want to chip away the leaves, after each step until you reach 1 or 2 nodes.
			- Its 1 or 2 nodes because eventually that's just how it is when you keep chipping leaves, either of those 2 nodes can be the root
		- important to use BFS because of how we are removing nodes and adding , so we want a queue
		```python
		from collections import deque
		class Solution(object):
		    def findMinHeightTrees(self, n, edges):
		        #using bfs
		        # find all neighbors
		        if n == 1:
		            return [0]
		        neis = {i:[] for i in range(n)}
		        degrees = {i: 0 for i in range(n)}
		        for a, b in edges:
		            neis[a].append(b)
		            neis[b].append(a)
		            degrees[a] += 1
		            degrees[b] += 1
		        q  = deque()
		        for key, val in degrees.items():
		            if val == 1:
		                q.append(key)
		        remaining = n
		        while remaining > 2:
		            length = len(q)
		            for _ in range(length):
		                remaining -= 1
		                node  = q.popleft()
		                for nei in neis[node]:
		                    degrees[nei] -=1
		                    if degrees[nei] == 1:
		                        q.append(nei)
		        return list(q)
		```
	- [Word Ladder](https://leetcode.com/problems/word-ladder/)
		- This is bfs but the hard part is figuring out the neighbors
			- Here we make buckets, where `buckets[x]` is an `[array of words]` , that fit the pattern x
			- x is something like `*og` or `d*g` 
			- Then we just put in the starting word in the queue together with the current distance as a pair
			- For the neighbors we look for every type of pattern we can 
		```python
		from collections import defaultdict, deque
		class Solution:
		    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
		        wordSet = set(wordList)
		        if endWord not in wordSet:
		            return 0
		        length = len(beginWord)
		        buckets = defaultdict(list)
		        for word in wordSet:
		            for i in range(length):
		                buckets[word[:i] +"*" + word[i+1:]].append(word)
		        q = deque()
		        q.append((beginWord, 1))
		        visited = set()
		        visited.add(beginWord)
		        while q:
		            word, dist = q.popleft()
		            if word == endWord:
		                return dist
		            for i in range(length):
		                neighbors = buckets[word[:i] +"*" + word[i+1:]]
		                for nei in neighbors:
		                    if nei not in visited:
		                        q.append((nei, dist+1))
		                buckets[word[:i] +"*" + word[i+1:]].clear()
		        return 0
		```
		
## Extra: Dijkstra's Algorithm
- Use this when you have to moving on a grid/graph has different costs each step
	- Look for questions that minimize the maximum ...
- Algorithm only works for non negative weights
- (E+V)log V runtime, E+V space
```python
def dalg(graph, start):
	# graph is a dictionary with graph[node] = array of [neighbor, weight]s
	# start is starting node
	# return dict where dict[node] is shortest distance from start
	pq = []
	heapq.heapify(pq)
	heapq.heappush(pq, (0, start))
	
	dist = {node:float("inf") for node in graph}
	dist[start] = 0
	
	while pq:
		curDist, popped = heapq.heappop(pq)
		
		
		if curDist > dist[popped]:
			continue
		for nei, weight in graph[popped]:
			if currDist + weight >= dist[nei]:
				continue
			else:
				dist[nei] = currDist + weight
				heapq.heappush(pq, (currDist+weight, nei))
	return dist
		
```
- Problems:
	- [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)
		- Same solution as always honestly
		```python
		class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        pq = []
        pq.append((0,0,0))
        rows = len(heights)
        cols = len(heights[0])
        effort = {(i,j):float("inf") for i in range(rows) for j in range(cols)}
        effort[(0,0)] = 0
        directions  = [(1,0), (-1, 0), (0,1), (0,-1)]
        while pq:
            curEffort, r, c  = heapq.heappop(pq)
            if curEffort > effort[(r,c)]:
                continue
            for dr, dc in directions:
                nr = r + dr
                nc = c + dc
                if nr in range(rows) and nc in range(cols):
                    newEdgeDiff = abs(heights[nr][nc] - heights[r][c])
                    if effort[(nr,nc)] > max(newEdgeDiff, effort[(r,c)]):
                        effort[(nr,nc)] = max(newEdgeDiff, effort[(r,c)])
                        heapq.heappush(pq, (effort[(nr,nc)], nr, nc))
        return effort[(rows-1, cols-1)]
		```
	- [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)
		- The question tries to trick you with time t, but it actually doesn't matter because you can move infinitely within a single time t
		- So the real question is asking for the path with the least elevation
	```python
	class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        pq = []
        pq.append((grid[0][0],0,0))
        rows = len(grid)
        cols = len(grid[0])
        elevations = {(i,j):float("inf") for i in range(rows) for j in range(cols)}
        elevations[(0,0)] = grid[0][0]
        directions = [(1,0) , (-1,0), (0,1), (0,-1)]
        while pq:
            currElev , r, c = heapq.heappop(pq)
            if currElev > elevations[(r,c)]:
                continue
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if nr in range(rows) and nc in range(cols):
                    checkMax = max(grid[nr][nc], currElev)
                    if checkMax < elevations[(nr,nc)]:
                        elevations[(nr,nc)] = checkMax
                        heapq.heappush(pq, (checkMax, nr, nc))
        return elevations[(rows-1, cols-1)]
	```
# 13. Matrix Traversal
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
	            popped = q.popleft()
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
	-  ```python
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

# 14. Back Tracking
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
                res.append(copy)
                return # so you dont uselessly loop 
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

# 15. Dynamic Programming
- Use this problem when trying to find the optimal substructure or overlapping subproblems
	- Optimal Substructure
		- Solution depends on solutions of its subroblems
			- ex. House Robber, solution depends on robbing previous houses
	- Overlapping Subproblems
		- The same subproblem appears again and again like in Fibonacci
		```
		fib(5)
		 ├─ fib(4)
		 │   ├─ fib(3)
		 │   │   ├─ fib(2)
		 │   │   └─ fib(1)
		 │   └─ fib(2)
		 └─ fib(3)
		     ├─ fib(2)
		     └─ fib(1)

		```
- There's a lot of different DP patterns to use
	- Fibonacci - > `dp[n] = dp[n-1] + dp[n-2]
	- Knapsack
	- Longest Common Subsequence
	- Longest Increasing Subsequence
	- Subset Sum
- Memoization vs Tabulation
	- Memoization 
		- Uses recursion and has a cache of some kind hash table (could be a table)
		- Also called top down approach
	- Tabulation
		- Iterative, the usual dp table
		- also called bottom up approach
- Sometimes especially for Fibonacci or some knapsacks you don't need to make a whole array, you just need the last 2 previous dp indexes or whichever indexes you need
	- do this if the previous indexes you need are consistent
	- do this to save memory
- Problems
	- [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
		- Simple Fibonacci question
		- Use first, second instead of array to save memory
		- its just second = second + first
	```python 
	class Solution:
	    def climbStairs(self, n: int) -> int:
	        first = 1
	        second = 1
	        res = 1
	        if n < 2:
	            return 1
	         
	        for i in range(n-1):
	            temp = second
	            second = second + first
	            first = temp
	        return second
	```
	- [House Robber](https://leetcode.com/problems/house-robber/)
		- Basically same as stairs problem, but not straight fibonacci.
		- Take or Skip DP pattern
		- Decide if you want to rob the current house then take `dp[n-2]` or don't take the house and take `dp[n-1]`
	```python
	class Solution:
    def rob(self, nums: List[int]) -> int:
        first = nums[0]
        if len(nums) < 2:
            return first
        second = max(nums[1], first)
        for i in range(len(nums)-2):
            temp = second
            second = max(second, first + nums[i+2])
            first = temp
        return second
	```
	- [Coin Change](https://leetcode.com/problems/coin-change/)
		- Here you want dp to have `dp[i]` be the minimum amount for amount = i, then build off that
		- For each amount i, you want to check the minimum amount of coins against using all the coin types available
	```python
	class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [-1]  * (amount+1) # better to just use float(inf) here 
        dp[0] = 0
        for i in range(amount+1):
            for j in range(len(coins)-1,-1,-1):
                coin = coins[j]
                # don't need this since when its i == coin, the last conditional
                # also catches this because of dp[0]
                # if i == coin:
                #     dp[i] = 1
                #     break
                if i - coin < 0:
                    continue
                if dp[i-coin] != -1:
                    dp[i] = min(dp[i-coin] + 1, dp[i]) if dp[i] != -1 else dp[i-coin] + 1
        return dp[amount]
	```
	- [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
		- Remember subsequence != substring
			- subsequences don't have to be consecutive, just after one another
		- You want dp to be 2D where i and j are the length of substrings of text1 and text2 
		- Your base cases are just when one or both substrings are 0, so its just 0
		- For finding `dp[i][j]` you want to check if the last index is the same, because those are the 2 you are adding/checking
			- if they are then you can increment from the previous i/j -1
			- If not just get the max from `[i-1][j] or [i][j-1]`
	```python 
	class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        #dp[i][j] checks longest subsequence for text1[0:i+1] and text2[0:j+1]
        dp = [[0] * (len(text2) + 1) for i in range(len(text1) + 1)] 
        # when i or j = 0, then thats just when there are no characters, so its 0
        for i in range(1,len(text1)+1):
            for j in range(1,len(text2)+1):
                # think of i and j as the length of text1 and text2
                # check the current character which would be i-1 or j-1 since i and j 
                # are lengths.
                # if they are the same we can extend our subsequence
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                # if its not the same, we look at the previous maxes, from when we had a shorter text1 or text2
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[len(text1)][len(text2)]
	```
	- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
		- Here the dp is the LIS until our index `dp[i]`
		- we need to check that last index against all previous indexes and check which one has the longest based on its dp
			- We of course need to check first if our index is larger
		- The return is important here
			- we want the max in our dp because our dp is based on including the last index 
			- so our last index might be smaller than the previous, so its a short subsequence
	```python
	class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * (len(nums))
        for i in range(1,len(dp)):
            # compare index i, with all previous numbers and if i is bigger, we can check
            # our currenet dp with dp[j] + 1 since dp[j] already has a LIS and i is bigger
            # than j
            for j in range(i):
                # This condition makes sure that dp[i] is where we NEED to have nums[i],
                # this means that the final answer might not be dp[-1], since we could
                # have nums[-1] be some really small number
                if nums[i] > nums[j]:
                    dp[i] = max(dp[j]+1, dp[i])

        return max(dp)
	```
	- [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
		- Dumbass name, its asking if you can split the list into 2 equal sum subsets
		- `dp[i]` is whether we can make the sum i with SOME elements
			- each dp is set to False initially except i = 0
			- which is basically guaranteed to use some since we are only going until half the sum of our total sum
		- One number at a time, we see what sums we can make
			- its important to iterate backwards because if we iterate forward we could be reusing the same number as a duplicate, which is not allowed
	```python 
	class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        
        target = sum(nums) // 2
        if sum(nums) %2 != 0:
            return False
        dp = [False] * (target+1)
        dp[0] = True
        for num in nums:
            for i in range(target, num-1,-1):
                dp[i] = dp[i] or dp[i - num]
        return dp[-1]

	```
	- [Word Break](https://leetcode.com/problems/word-break/)
		- Quite simple but just make sure you get why the dp length is len +1
		- It might be confusing for the indexing in the loop i because of our dp length, but just think of each index in dp as up until i (exclusive) its possible to make this using wordDict
		- In the dfs approach, we need to use memo as a tracker to avoid redoing dfs(i) on things we've already run or else its exponential time
			- The memo at the end just means we failed to reach the end of the string, so its impossible from that index, so set memo[i] to false
		```python
		class Solution:
		    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
		        dp = [False] * (len(s) + 1)
		        dp[0] = True
		        for i in range(len(s)+1):
		            if dp[i] == True:
		                for word in wordDict:
		                    if i + len(word) <= len(s) and s[i:i+len(word)] in wordDict:
		                        dp[i+len(word)] = True
		                        if dp[-1] == True:
		                            return True
		        return dp[-1]
		# DFS approach
		class Solution:
		    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
		        memo  = {}
		        def dfs(i):
		            if i == len(s):
		                return True
		            if i in memo:
		                return memo[i]
		            for word in wordDict:
		                if i + len(word) <= len(s) and s[i:i + len(word)] == word:
		                    if dfs(i+len(word)):
		                        memo[i] = True
		                        return True 
		            memo[i] = False
		            return False
		        return dfs(0)
		```
	- [Unique Paths](https://leetcode.com/problems/unique-paths/)
		- This is a problem solved using a top down approach
		- When doing top down you want to have a cache so you don't keep repeating function calls
		```python
		class Solution:
		    def uniquePaths(self, m: int, n: int) -> int:
		        cache = {}
		        def dfs(r,c):
		            if r== m-1 and c == n-1:
		                return 1
		            if r not in range(m) or c not in range(n):
		                return 0
		            if (r,c) in cache:
		                return cache[(r,c)]
		            total = dfs(r+1,c) + dfs(r, c+1)
		            cache[(r,c)] = total
		            return total
		        return dfs(0,0)
		```
	- [Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)
		- This is a problem that requires understanding of overlapping intervals (but not much) and DP
		- `dp[i]` is max profit for jobs until index i
		- Here we want to sort by end times because since we want our dp to be until index i, then its weird if we sort by start and that job at i ends really late or some earlier job ends later than that job
		- Here we use binary search to find the last non overlapping job, to avoid iterating over the whole array again and again
			- Allows for O(nlogn) time instead of O(n^2)
		```python
		class Solution:
		    def jobScheduling(self, startTime: List[int], endTime: List[int], profit: List[int]) -> int:
		        jobs = sorted(zip(startTime, endTime, profit), key=lambda x:x[1])
		        ends = [e for _,e,_ in jobs]
		        dp = [0]*len(jobs)
		        def find_last_non_overlap(start):
		            l,r = 0, len(ends)-1
		            res=  -1
		            while l<=r:
		                mid = (l+r)//2
		                if ends[mid] <= start:
		                    res = mid
		                    l = mid +1
		                else:
		                    r = mid -1
		            return res
		        for i, (s,e,p) in enumerate(jobs):
		            last = find_last_non_overlap(s)
		            take = p+ dp[last] if last != -1 else p
		            skip = dp[i-1] if i> 0 else 0
		            dp[i] = max(take, skip)
		        return dp[-1]
		```
	- ## Below are mostly interval DP questions
	- [Burst Balloons](https://leetcode.com/problems/burst-balloons/)
		- The trick is to work from the smallest intervals first, so 3 coin intervals are essentially the base case
		- The k in the k loop is imagined as the last balloon you burst
			- you don't do `nums[k-1]*nums[k]*nums[k+1]` because k-1 and k+1 could have burst already
		- Also note that `dp[l][r]` is exclusive on both sides
		- Eventually we will build up until our largest range which is the original nums
			- note we also padded the nums array with 1s on the ends, to account for going out of bounds
		```python 
		class Solution:
		    def maxCoins(self, nums: List[int]) -> int:
		        nums = [1] +nums + [1]
		        dp = [[0] * len(nums) for i in range(len(nums))]
		        for length in range(2,len(nums)):
		            for l in range(len(nums)-length):
		                r = l+ length
		                for k in range(l+1, r):
		                    dp[l][r] = max(dp[l][r], nums[l]*nums[k]*nums[r] + dp[l][k] + dp[k][r])
		        return dp[0][len(nums)-1]

		```
	- [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
		- just track if our inner interval `dp[l+1][r-1]` is a palindrome, if it is and our l+1 <= r- 1are correct, then check for palindrome property
		```python
		class Solution:
		    def countSubstrings(self, s: str) -> int:
		        dp = [[False]*len(s) for _ in range(len(s))]
		        count = 0
		        for length in range(len(s)):
		            for l in range(len(s)-length):
		                r= l + length
		                if l + 1 <= r-1 and dp[l+1][r-1] == False:
		                    continue
		                if s[l] == s[r]:
		                    count += 1
		                    dp[l][r] = True
		        return count
		```
	- [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
		- Very similar to previous except we don't check if inner interval is palindrome, since it doesn't have to be
		- If `s[l] == s[r]` just add 2 to the inner interval, else just get the maximum of skipping l or r
			- It also accounts skipping both when you check those dps 
			```python
			class Solution:
			    def longestPalindromeSubseq(self, s: str) -> int:
			        dp = [[0]* len(s) for _ in range(len(s))]
			        longest = 0
			        for length in range(len(s)):
			            for l in range(len(s)-length):
			                r = l + length
			                if s[l] == s[r]:
			                    if l == r:
			                        dp[l][r] = 1
			                    else:
			                        dp[l][r] = 2 + dp[l+1][r-1]
			                        longest = max(dp[l][r], longest)
			                else:
			                    dp[l][r] = max(dp[l+1][r], dp[l][r-1])
			        return dp[0][len(s)-1]
			```
# Sorting Problems
- Some problems require unique sorting that will run in O(n) time instead of the nlogn time for quick, heap or merge sorts
- Bucket Sort
	- 2 kinds,\
		- index of the array is the actual value of the num
		- index is the frequency
	- Use this when the max num is small or if the frequency has a small range
	- for the 2 kinds pick based on question
		- if its asking for most frequent use index by frequency
		- if its asking for simple counting just use index by num
- Problems
	- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) using Bucket Sort
		- Use frequency indexed so you can iterate backwards from highest freq
		```python
		class Solution:
		    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
		        counter = {}
		        res = []
		        buckets = [[] for i in range(len(nums)+1)]
		       
		        for num in nums:
		            counter[num] = counter.get(num, 0) + 1
		        for num, freq in counter.items():
		            buckets[freq].append(num)
		        for i in range(len(buckets)-1,-1,-1):
		            for num in buckets[i]:
		                    
		                res.append(num)
		                k -= 1
		                if k <= 0:
		                    return res
		```
	- [Sort Colors](https://leetcode.com/problems/sort-colors/)
		- This question is asking to arrange an array of 0s, 1s, and 2s in order by changing the input array itself
		- Use the Dutch National Flag Algorithm
		- The Dutch national flag algorithm involves sorting the `nums` array by partitioning it into 3 segments.
		- `nums[0]...nums[low - 1]` : This part should consist of **all zeroes**.
		- `nums[low]...nums[mid - 1]` : This part should consist of **all ones**.
		- `nums[mid]...nums[end of array]` : This part should consist of **all twos**.
		```python
		class Solution:
		    def sortColors(self, nums: List[int]) -> None:
		        low, mid = 0,0 
		        high = len(nums)-1
		        while mid <= high:
		        #if 0 then swap with low and increment both mid and low
		            if nums[mid] == 0:
		                temp = nums[low]
		                nums[low] = nums[mid]
		                nums[mid] = temp
		                mid+= 1
		                low += 1
		        #if 1 then just increment mid
		            elif nums[mid] == 1:
		                mid += 1
		        #if 2 then decrement only high and swap mid and high
		            else:
		                temp = nums[high]
		                nums[high] = nums[mid]
		                nums[mid] = temp
		                high -=1
		```
# Disjoin Set Union (Union Find)
- This algorithm is for questions, where you have a bunch of items/nodes that belong to the same group as another item, given some piece of information/connecting factor
	- When asked if some pair belongs together, use DSU
	- Used to find redundant edges (loops) too
- Ex. accounts sharing an email (there is a root email connecting all of them)
- Implementation of DSU:
	```python 
	class DSU:
	    def __init__(self):
	        self.parent = {} # root of item
	        self.rank = {} # height
	    def find(self, x):
	        if x not in self.parent: # if x has not been seen
	            self.parent[x] = x
	            self.rank[x] = 0
	        if self.parent[x] != x: # recursively find parent
	            self.parent[x] = self.find(self.parent[x])
	        return self.parent[x]
	    def union(self, a,b):
	        ra = self.find(a)
	        rb = self.find(b)
	        if ra == rb:
	            return
	        if self.rank[ra] < self.rank[rb]: #we want ra to be taller
	            ra,rb= rb,ra
	        self.parent[rb] = ra
	        if self.rank[ra] == self.rank[rb]: #only increase height if same rank
	            self.rank[ra] += 1
	```
- Problems:
	- [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
		- This basically purely DSU
		```python
		class Solution:
		    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
		        dsu = DSU()
		        email_to_name = {}
		        for account in accounts:
		            name = account[0]
		            # make first email the root
		            firstEmail = account[1]
		            for email in account[1:]:
		                email_to_name[email] = name
	#this union will catch the common email, since firstEmail will eventually be 
	#absorbed into the bigger set if it ever encounters it
		                dsu.union(firstEmail, email)
		        groups = {}
		#Everything here is just making the actual result
		        for email in email_to_name:
		            root = dsu.find(email)
		            if root not in groups:
		                groups[root] = []
		            groups[root].append(email)
		        result = []
		        for root, email in groups.items():
		            name = email_to_name[root]
		            result.append([name]+ sorted(email))
		        return result

		```
	- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
		- This is a way of doing DSU without having to fully implement the class. Its slightly modified to fit the question better but its essentially the same (only the return of union was changed)
		```python
		class Solution:
		    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
		        par = [i for i in range(len(edges)+1)]
		        rank = [1 for i in range(len(edges)+1)]
		        def find(node):
		            if par[node] != node:
		                par[node] = find(par[node])
		            return par[node]
		
		        def union(n1, n2):
		            p1 = find(n1)
		            p2 = find(n2)
		
		            if p1 == p2:
		                return False
		            if rank[p1] > rank[p2]:
		                par[p2] = p1
		                rank[p1] += rank[p2]
		            else:
		                par[p1] = p2
		                rank[p2] += rank[p1]
		            return True
		        for n1,n2 in edges:
		            if not union(n1,n2):
		                return [n1,n2]
		```
# String Processing
- Generally just questions where you process strings
- Problems:
	- [Basic Calculator](https://leetcode.com/problems/basic-calculator/)
		- idk wat to say about this
		```python
		class Solution:
		    def calculate(self, s: str) -> int:
		        res =0 
		        num =0
		        sign = 1
		        stack = []
		
		        for c in s:
		            if c.isdigit():
		                num = num*10 + int(c)
		            elif c == "+" or c == "-":
		                res += sign*num
		                num = 0
		                sign = 1 if c =="+" else -1
		            elif c == "(":
		                stack.append((res, sign))
		                res =0
		                num =0 
		                sign =1
		            elif c== ")":
		                res += sign*num
		                num =0 
		                resPrev, signPrev = stack.pop()
		                res = resPrev + (signPrev *res)
		
		        return res +sign * num   
		```
# Bit Manipulation
- XOR property
	- Its commutative and associative 
		- commutative: `a+b=b+a, a x b = b x a`
		- associative: `(a + b) + c = a + (b + c), (a x b) x c = a x (b x c)`
- Problems:
	- [136. Single Number](https://leetcode.com/problems/single-number/)
		- Array has numbers, each are a pair of the same number except one, get the one
		- XOR starting with 0 (so first number stays as is)
		- works because of commutation and association
			```python
			class Solution:
			    def singleNumber(self, nums: List[int]) -> int:
			        final = 0
			        for num in nums:
			            final = final ^ num
			        return final
			```
	- 