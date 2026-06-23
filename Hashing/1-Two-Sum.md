# LeetCode 1: Two Sum
## Problem Statement
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`.
You may assume that each input has exactly one solution, and you may not use the same element twice.
---
# Approach 1: Brute Force
## Idea
Check every possible pair of elements and see if their sum equals the target.
## Algorithm
1. Iterate through each element using index `i`.
2. For each `i`, iterate through the remaining elements using index `j`.
3. If `nums[i] + nums[j] == target`, return `[i, j]`.
## Code
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)

        for i in range(n - 1):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    return [i, j]
```
## Dry Run
Input:
```python
nums = [2, 7, 11, 15]
target = 9
```
Iterations:
* i = 0, j = 1
* nums[0] + nums[1] = 2 + 7 = 9
* Return `[0, 1]`
## Complexity Analysis
### Time Complexity
* Outer loop runs `n` times.
* Inner loop runs approximately `n-1`, `n-2`, ..., `1` times.
Total operations:
```
(n-1) + (n-2) + ... + 1
= n(n-1)/2
```
Therefore,
**Time Complexity: O(n²)**
### Space Complexity
No extra data structure is used.
**Space Complexity: O(1)**
---
# Approach 2: Hash Map (Optimal)
## Idea
For every number, calculate the value needed to reach the target.
```
remaining = target - nums[i]
```
If `remaining` has already been seen, we have found our answer.
Otherwise, store the current number and its index in a hash map.
## Algorithm
1. Create an empty hash map.
2. Traverse the array once.
3. Compute `remaining = target - nums[i]`.
4. If `remaining` exists in the hash map, return the indices.
5. Otherwise, store the current element and its index.
## Code
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash_map = {}

        for i in range(len(nums)):
            remaining = target - nums[i]

            if remaining in hash_map:
                return [hash_map[remaining], i]

            hash_map[nums[i]] = i
```
## Dry Run
Input:
```python
nums = [2, 7, 11, 15]
target = 9
```
### Iteration 1
```
i = 0
nums[i] = 2
remaining = 7
hash_map = {}
```
7 is not in the hash map.
Store:
```
hash_map = {2: 0}
```
### Iteration 2
```
i = 1
nums[i] = 7
remaining = 2
hash_map = {2: 0}
```
2 exists in the hash map.
Return:
```python
[0, 1]
```
---
# Complexity Analysis
### Time Complexity
* Single traversal of the array → `O(n)`
* Hash map insertion → `O(1)` average.
* Hash map lookup → `O(1)` average.
Therefore,
**Time Complexity: O(n)**
### Space Complexity
The hash map may store up to `n` elements.
**Space Complexity: O(n)**
---
# Comparison
| Approach    | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Brute Force | O(n²)           | O(1)             |
| Hash Map    | O(n)            | O(n)             |
---
# Key Takeaway
Whenever a problem asks:
* Find a pair
* Find a complement
* Check if a value already exists
Think about using a **Hash Map** to reduce the time complexity from **O(n²)** to **O(n)**.
