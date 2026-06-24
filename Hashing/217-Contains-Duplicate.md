# 217. Contains Duplicate

## Pattern

HashSet / Seen Before

## Problem

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

---

## Constraints

```text
1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
```

---

# Approach 1: Brute Force

## Idea

Compare every element with every other element.

## Algorithm

1. Pick an element at index `i`.
2. Compare it with every element after it.
3. If two elements are equal, return `True`.
4. If no duplicates are found, return `False`.

## Code

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        n = len(nums)

        for i in range(n - 1):
            for j in range(i + 1, n):
                if nums[i] == nums[j]:
                    return True

        return False
```

## Dry Run

```python
nums = [1, 2, 3, 1]
```

```text
i = 0, j = 1 -> 1 != 2
i = 0, j = 2 -> 1 != 3
i = 0, j = 3 -> 1 == 1
Return True
```

## Complexity Analysis

### Time Complexity

```text
(n-1) + (n-2) + ... + 1
= n(n-1)/2
= O(n²)
```

### Space Complexity

```text
O(1)
```

---

# Approach 2: Sorting

## Idea

After sorting, duplicate elements become adjacent.

## Algorithm

1. Sort the array.
2. Traverse the array once.
3. If two adjacent elements are equal, return `True`.
4. Otherwise, return `False`.

## Code

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums.sort()

        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return True

        return False
```

## Dry Run

```python
nums = [1, 2, 3, 1]
```

After sorting:

```python
[1, 1, 2, 3]
```

```text
nums[1] == nums[0]
Return True
```

## Complexity Analysis

### Time Complexity

```text
Sorting : O(n log n)
Traversal : O(n)

Total : O(n log n)
```

### Space Complexity

```text
O(1)
```

(ignoring Python's internal sorting space requirements)

---

# Approach 3: HashSet (Optimal)

## Key Observation

We do not need to know how many times a number appears.

We only need to know:

```text
"Have I seen this number before?"
```

A HashSet gives:

```text
Insertion -> O(1)
Lookup    -> O(1)
```

---

## Idea

1. Create an empty HashSet.
2. Traverse the array.
3. If the current element is already in the set, return `True`.
4. Otherwise, add it to the set.
5. If the traversal finishes, return `False`.

## Code

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hash_set = set()

        for num in nums:
            if num in hash_set:
                return True

            hash_set.add(num)

        return False
```

## Dry Run

```python
nums = [1, 2, 3, 1]
```

```text
hash_set = {}

num = 1
hash_set = {1}

num = 2
hash_set = {1, 2}

num = 3
hash_set = {1, 2, 3}

num = 1
1 already exists in hash_set

Return True
```

---

## Complexity Analysis

### Time Complexity

```text
Traversal : O(n)
HashSet lookup : O(1) average
HashSet insertion : O(1) average

Overall : O(n)
```

### Space Complexity

```text
O(n)
```

because in the worst case all elements are distinct.

---

# Comparison

| Approach    | Time       | Space |
| ----------- | ---------- | ----- |
| Brute Force | O(n²)      | O(1)  |
| Sorting     | O(n log n) | O(1)  |
| HashSet     | O(n)       | O(n)  |

---

# Interview Flow

```text
Brute Force
        ↓
Sorting
        ↓
HashSet (Optimal)
```

Mention all three approaches, but implement the HashSet solution.

---

# Learnings

* If the question asks:

  * Have we seen this before?
  * Does an element already exist?
  * Are there duplicates?

  Think about using a HashSet.

* HashSet is often used to replace repeated linear searches.

* A little extra space can reduce time complexity significantly.

---

# Tags

* Array
* Hashing
* HashSet
