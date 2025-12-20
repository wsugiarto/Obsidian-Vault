
# Prefix Sum
![[Pasted image 20251218163244.png]]
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