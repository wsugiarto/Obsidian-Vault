
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
		- make a hash table to keep track of the last time you 