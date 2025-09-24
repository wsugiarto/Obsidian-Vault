1. How to Identify
    - Usually when question is asking to maximize or minimize something under a certain arrangement or condition
    - Also used for questions about overlapping subproblems or optimal substructures
    - Generally subsequence problems are usually DP, just make sure you sort things if you need to.
2. Approach:
    1. Since we usually use some kind of array or dictionary, we need to make each index meaningful, so find out what the index will mean
    2. Next find the appropriate relationship from one index to another.
        1. ie. Find how n+1 can be derived from n or n-1 or anything before/after it.
    3. Optimization note:
        1. Make sure to use some kind of object to store values you already calculated to reduce time complexity.

General Notes:

- Subarray : Contiguous array inside the big array
- Subsequence: Subarray but doesn’t have to be contiguous
- Working backwards is often more helpful than forwards on the array
- If working on the given array as indices don’t work, you can try going from 0 to your target if lets say its asking for smallest combination of something to reach that target

Examples:

1. House Robber 2
    
    1. Make a helper which just for loop on num and for each u get max for num + 2 houses before or just skipping num so value from previous house
2. Longest palindromic Subtring
    
    1. Honestly just a 2 points problem
    2. Iterate through character in string, this will be our center of substring, so expand l and r until you can’t, while checking s[l] =s[r], then if its the longest just make that the new res
    3. Make sure to check even and odd length palindromes, so start l,r = i,i and l,r = i,i+1 too
3. Palindromic Substrings
    
    1. Same approach as 2, except we just need to add 1 to the count, when we find a new palindrome.
4. Coin Change
    
    1. The subproblems are checking the number of combinations of amounts less than the target, then checking
        1. for each amount you need to checkp the dp[i-coin] until dp[0]
5. Maximum Product Subarray
    
    1. Need to account for negative maxes and positive maxes, so max and min
    2. iterate over each num the new curMax = max(n * curMax, n * curMin, n) and curMin = min(tmp, n* curMin, n) and res = max(curMax, res)
        1. Make sure when u see a zero reset curMax and min to 1
6. Word Break
    
    1. We work backwards with dp = [False] * len(s) +1
    2. make the dp[len(s) = true
    3. iterate s backwards, then check each word in the dictionary:
        1. if i+ length of the word is ≤ length of s and s[i:i+len(word)]== word, then check dp[i+len(word)]
        2. then check if that next dp[i] is true
        3. finally just check dp[0]
7. Longest Increasing Subsequence
    
    1. dp = [1]* len(nums) for base case
    2. iterate array backwards
        1. check each value after index i (nums[i] < nums[j]) and if it is get the max between dp[i] and 1+ dp[j]
    3. Return the max dp since its not necessarily dp[0]
8. Partition Equal Subset Sum
    
    1. The target is half of the total, so check if sum is odd
    2. Basically we create a set adding 0, then we iterate over our array and add num[i] to each value of the set, while maintaining previous values, do this for every num in nums. If target is ever in set we return true.
    
    ```java
    class Solution(object):
        def canPartition(self, nums):
            if sum(nums) % 2 ==1 :
                return False
            dp = set()
            dp.add(0)
            target = sum(nums)/2
            for i in range(len(nums)-1,-1,-1):
                nextDP = set()
                for t in dp:
                    if((t+nums[i]) == target):
                        return True
                    nextDP.add(t)
                    nextDP.add(t + nums[i])
                dp = nextDP
            return True if target in dp else False
    ```