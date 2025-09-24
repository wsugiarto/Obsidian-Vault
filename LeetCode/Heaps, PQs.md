How to Identify:

- Very Often:
    - Kth largest/smallest/most something element
        - Approach using a min/max heap to pop and push

Examples:

1. Kth Largest Element in a Stream
    1. Use min heap of size k, since we just get the min in O(1) time which is the kth largest
2. Last Stone Weight
    1. Use a max heap and pop 2, then combine and re-push
3. K Closest Points to Origin
    1. just push into a min heap, with \[dist, x, y], then start popping k times
4. Task Scheduler
    1. Count all the different tasks, make a new array with the number of each tasks and make a max heap using that. Also keep track of time.
    2. Pop the max from the max heap and push in a queue with the time it can be put back on the heap as long as the count > 0.
    3. Push back into heap when the time is same as the time in the queue
5. Design Twitter
    1. init
        1. Make a global counter/time = 0 to keep track of when tweets were posted
        2. followMap = dictionary of sets
        3. tweetMap = dictionary of arrays of \[count, tweetId]
    2. postTweet
        1. append the tweet to tweetMap\[userId] and decremenet count since we using max heap later
    3. getNewsFeed
        1. add user to their own follow list
        2. for each followee push into our heap \[count, tweetId, followeeId, index-1, where index starts at the last index of the tweet array of the followee
        3. While heap is not empty and the length of our result is less than 10, we pop form our heap and append the tweetId. Then if that followee still has tweets (index ≥0) then push the next tweet into the heap
    4. follow
        1. add to followmap\[user]
    5. unfollow
        1. remove from followmap\[user]
6. Find Median from Data Stream
    1. Make 2 heaps one for the first half (max heap) and the second half(min heap)
    2. addNum
        1. make sure max of first heap is less than min of second
        2. make sure they are at most 1 length away from each other
    3. findMedian
        1. get the max/min depneding on which one is longer, or division if same size.

Notes:

- Heapify is O(n)
    - heapq.heapify(your_array) in python is going to give you a min heap
    - In Python to get a max heap, convert all values to negatives then heapify
    - in an array like \[[1,2], [2,3]], heapify will look at the first value in each array entry
- heap pop is O(logn)
    - heapq.heappop(your_array) in python
    - heapq.heappush(your_array, your number) in python