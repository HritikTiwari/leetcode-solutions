# 128. Longest Consecutive Sequence
## Pattern
HashSet / Sequence Detection
## Problem
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
You must write an algorithm that runs in **O(n)** time.
---
## Constraints
```text
0 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
```
---
# Approach 1: Brute Force
## Idea
For every element, keep checking whether the next consecutive number exists in the array.
## Algorithm
1. Pick an element `x`.
2. Check if `x+1` exists.
3. If yes, increment the count and continue.
4. Repeat for every element.
## Code
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
## Complexity Analysis
### Time Complexity: O(n²)
The operation:
```python
num + 1 in nums
```
takes `O(n)` because `nums` is a list.
In the worst case:
```python
nums = [1,2,3,4,5,...,n]
```
we repeatedly search the array for the next element, leading to:
```text
n + (n-1) + (n-2) + ... + 1
= O(n²)
```
### Space Complexity: O(1)
No extra data structure is used.
---
# Approach 2: Sorting
## Approach 2: Sorting

### Idea

If we sort the array, all consecutive numbers become adjacent.

For example:

```text
Original:
[100, 4, 200, 1, 3, 2]

Sorted:
[1, 2, 3, 4, 100, 200]
```

Now, instead of searching for the next consecutive number, we only need to compare the current element with the previous unique element.

There are three possible cases:

1. **Current number is consecutive**

   * If `num == last_smaller + 1`, extend the current sequence.

2. **Current number is a duplicate**

   * Ignore it because duplicates neither start nor extend a sequence.

3. **Current number starts a new sequence**

   * Reset the count to `1`.

The variable `last_smaller` always stores the last unique element that was processed.

---

### Algorithm

1. Sort the array.
2. Initialize:

   * `count = 0`
   * `longest = 0`
   * `last_smaller = -∞`
3. Traverse the sorted array.
4. For every number:

   * If it is exactly one greater than `last_smaller`, extend the sequence.
   * Else if it is different from `last_smaller`, start a new sequence.
   * Otherwise, it is a duplicate, so ignore it.
5. Update the maximum sequence length after each iteration.
6. Return the longest sequence length.

---

### Code

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums.sort()

        longest = 0
        last_smaller = float(-inf)
        count = 0

        for num in nums:

            if num - 1 == last_smaller:
                count += 1
                last_smaller = num

            elif num != last_smaller:
                count = 1
                last_smaller = num

            longest = max(longest, count)

        return longest
```

---

### Example

```text
nums = [1, 2, 2, 3, 4]
```

| Current | last_smaller | Action             | Count | Longest |
| ------- | ------------ | ------------------ | ----: | ------: |
| 1       | -∞           | Start new sequence |     1 |       1 |
| 2       | 1            | Consecutive        |     2 |       2 |
| 2       | 2            | Duplicate → Ignore |     2 |       2 |
| 3       | 2            | Consecutive        |     3 |       3 |
| 4       | 3            | Consecutive        |     4 |       4 |

Answer:

```text
4
```

## Dry Run
```python
nums = [100,4,200,1,3,2]
```
After sorting:
```python
[1,2,3,4,100,200]
```
Longest sequence:
```text
1 → 2 → 3 → 4
Length = 4
```
## Complexity Analysis
### Time Complexity
```text
Sorting = O(n log n)
Traversal = O(n)
Total = O(n log n)
```
### Space Complexity
```text
O(1)
```
(ignoring Python's internal sorting space requirements).
---
# Approach 3: HashSet (Optimal)
## Key Observation
Every consecutive sequence has exactly one starting point.
For example:
```text
1,2,3,4
```
The sequence starts only at `1`.
Because:
```text
1-1 = 0 (does not exist)
2-1 = 1 (exists)
3-1 = 2 (exists)
4-1 = 3 (exists)
```
So we should start counting only when:
```python
x - 1 not in my_set
```
---
## Idea
1. Put all numbers into a set.
2. Find numbers that are sequence starters.
3. Expand the sequence from those starters only.
## Code
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        my_set = set(nums)
        longest = 0

        for x in my_set:

            if x - 1 not in my_set:
                count = 1

                while x + 1 in my_set:
                    x += 1
                    count += 1

                longest = max(longest, count)

        return longest
```
---
## Dry Run
```python
nums = [100,4,200,1,3,2]
```
Set:
```python
{100,4,200,1,2,3}
```
### x = 1
```text
0 not in set
```
Start sequence:
```text
1 → 2 → 3 → 4
Length = 4
```
### x = 2
```text
1 exists
```
Skip.
### x = 3
```text
2 exists
```
Skip.
### x = 4
```text
3 exists
```
Skip.
Answer:
```text
4
```
---
## Complexity Analysis
### Time Complexity: O(n)
Although there is a nested `while` loop, each element is visited only once.
For example:
```text
1 → 2 → 3 → 4
```
is traversed only when `x = 1`.
When:
```text
x = 2, 3, 4
```
we skip them because:
```python
x - 1 in my_set
```
Therefore, every number participates in the `while` loop at most once.
Total:
```text
Building set : O(n)
Traversal    : O(n)
Overall      : O(n)
```
### Space Complexity
```text
O(n)
```
because the HashSet may contain all elements.
---

# Comparison

| Approach    | Time       | Space |
| ----------- | ---------- | ----- |
| Brute Force | O(n²)      | O(1)  |
| Sorting     | O(n log n) | O(1)  |
| HashSet     | O(n)       | O(n)  |
---
# Learnings
* HashSet provides `O(1)` average lookup.
* Repeated linear searches often indicate that hashing can help.
* Sometimes the optimization comes from reducing the number of starting points instead of optimizing the inner loop.
* A nested loop does not automatically mean `O(n²)`.
---
# Tags
* Array
* Hashing
* HashSet
* Sequence Detection
