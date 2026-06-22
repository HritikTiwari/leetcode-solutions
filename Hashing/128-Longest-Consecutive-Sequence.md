# 128. Longest Consecutive Sequence
## Pattern
HashSet / Sequence Detection
## Problem
https://leetcode.com/problems/longest-consecutive-sequence/
---
## Constraints
0 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
## Approach 1: Brute Force
### Idea
Iterate through each element and keep checking if the next number exists in array.
### Code
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        n = len(nums)
        max_count = 0
        for i in range(n):
            num = nums[i]
            count = 1
            while num + 1 in nums:
                count += 1
                num += 1
            max_count = max(max_count, count)
        return max_count
```
### Complexity
Time: O(n²)
Space: O(1)

### Problem
requires checking the entire array every time, causing TLE on submission.
---
## Approach 2: Sorting 
### Idea
First sort the array, then find the last smaller element while iterating, then return the longest.

### Code
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums.sort()
        longest = 0
        last_smaller = float(-inf)
        count = 0
        for i in range(len(nums)):
            num = nums[i]
            if num-1 == last_smaller:
                count+=1
                last_smaller = num
            elif num!=last_smaller:
                count = 1
                last_smaller = num
            longest = max(longest, count)
        return longest
```
### Complexity
Time: O(nlogn + n)
Space: O(1)

### Why Improve?
Sorting gives O(n log n), but the problem asks for an O(n) solution.
---
## Approach 3: Set operations to reduce time complexity
### Idea
First create a set, then after finding the smallest element, return the longest.
### Key Observation
Every sequence has exactly one starting point.
If x-1 exists, then x cannot be the beginning of a new sequence.
### Code
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        my_set = set()
        longest = 0
        for i in range(len(nums)):
            my_set.add(nums[i])
        for j in my_set:
            x = j
            if x-1 not in my_set:
                count = 1
                while x+1 in my_set:
                    count += 1
                    x += 1
                longest = max(longest, count)
        return longest
```
### Complexity
Time: O(n)
Space: O(n)
## Learnings
- HashSet provides O(1) average lookup.
- Always look for repeated linear searches.
- Sometimes the key is not optimizing the inner loop, but reducing the number of starting points.
## Tags
Hashing
Arrays
Sequences
