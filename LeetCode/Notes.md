
# Prefix Sum
![[Pasted image 20251218163244.png]]
- Making an array where array[i] is the sum of all values until index i
- P[j] - P[i-1]
- Problems
	- When you make the prefix array make it len(n) +1 so its easier to do the P[j] - P[i-1] formula
	- Range Sum Query
		- 