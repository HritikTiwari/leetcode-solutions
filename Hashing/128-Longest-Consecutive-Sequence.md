# 128. Longest Consecutive Sequence

## Problem
https://leetcode.com/problems/longest-consecutive-sequence/
---

## Approach 1: Brute Force

### Idea
Iterate through each element and keep checking if the next number exists in array.

### Code
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

### Complexity
Time: O(n²)
Space: O(1)

### Problem
requires checking the entire array every time, causing TLE on submission.

---
