
# Prefix Sum
![[Pasted image 20251218163244.png]]
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
			- You can do this by making a hash table