# 347. Top K Frequent Elements

## Pattern

HashMap + Bucket Sort

## Problem

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

You may return the answer in any order.

---

## Constraints

```text
1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= number of unique elements
```

---

# Approach 1: Sorting

## Idea

Count the frequency of every element, then sort the elements according to their frequencies in descending order.

---

## Algorithm

1. Build a frequency map.
2. Sort the map by frequency.
3. Return the first `k` elements.

---

## Code

```python
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)

        return [num for num, _ in sorted(
            count.items(),
            key=lambda x: x[1],
            reverse=True
        )[:k]]
```

---

## Complexity

### Time

```text
Counting : O(n)
Sorting  : O(n log n)

Overall : O(n log n)
```

### Space

```text
O(n)
```

---

# Why Improve?

Sorting every unique element is unnecessary.

We only need the **top k** elements.

---

# Approach 2: Min Heap

## Idea

Keep only the `k` most frequent elements inside a min heap.

Whenever the heap size exceeds `k`, remove the smallest frequency.

---

## Complexity

### Time

```text
Build frequency map : O(n)

Heap operations :
O(n log k)

Overall :
O(n log k)
```

### Space

```text
O(n)
```

(or `O(k)` for the heap, excluding the frequency map)

---

# Why Improve?

Can we do even better?

Notice:

The maximum frequency of any number cannot exceed `len(nums)`.

That means frequencies range from:

```text
1 ... n
```

Instead of sorting, we can place every number directly into a bucket representing its frequency.

---

# Approach 3: Bucket Sort (Optimal)

## Key Observation

If a number appears:

```text
3 times
```

place it into:

```python
bucket[3]
```

Then simply traverse the buckets from highest frequency to lowest.

No sorting is required.

---

## Algorithm

1. Count the frequency of every number.
2. Create `n + 1` buckets.
3. Store each number in the bucket corresponding to its frequency.
4. Traverse buckets from highest frequency to lowest.
5. Collect elements until `k` elements are obtained.

---

## Code

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freq = [[] for _ in range(len(nums) + 1)]
        count = {}

        for n in nums:
            count[n] = count.get(n, 0) + 1

        for n, c in count.items():
            freq[c].append(n)

        result = []

        for i in range(len(freq) - 1, 0, -1):
            for n in freq[i]:
                result.append(n)

                if len(result) == k:
                    return result
```

---

## Dry Run

```python
nums = [1,1,1,2,2,3]
k = 2
```

### Step 1

Frequency map

```python
{
    1:3,
    2:2,
    3:1
}
```

---

### Step 2

Buckets

```text
index : elements

0 : []
1 : [3]
2 : [2]
3 : [1]
4 : []
5 : []
6 : []
```

---

### Step 3

Traverse backwards

```text
bucket[6]

↓

bucket[5]

↓

bucket[4]

↓

bucket[3]
```

Take:

```text
1
```

Result

```python
[1]
```

Continue

```text
bucket[2]
```

Take

```text
2
```

Result

```python
[1,2]
```

Collected `k = 2` elements.

Return:

```python
[1,2]
```

---

## Complexity Analysis

### Time

```text
Count frequencies : O(n)

Fill buckets : O(n)

Traverse buckets : O(n)

Overall : O(n)
```

---

### Space

```text
Frequency map : O(n)

Buckets : O(n)

Overall : O(n)
```

---

# Why Bucket Sort Works

The highest possible frequency is:

```text
len(nums)
```

Therefore,

instead of sorting frequencies,

we can directly store numbers into frequency-indexed buckets.

This removes the need for sorting entirely.

---

# Comparison

| Approach    | Time       | Space |
| ----------- | ---------- | ----- |
| Sorting     | O(n log n) | O(n)  |
| Min Heap    | O(n log k) | O(n)  |
| Bucket Sort | **O(n)**   | O(n)  |

---

# Interview Flow

```text
Sorting

↓

Min Heap

↓

Bucket Sort (Optimal)
```

Implement the **Bucket Sort** solution.

---

# Learnings

* HashMaps are useful for counting frequencies.
* When the maximum key range is known (`1...n`), Bucket Sort can replace sorting.
* Heap is useful when only the top `k` elements are needed.
* Bucket Sort can outperform heaps when the frequency range is bounded.

---

# Tags

* Array
* Hashing
* Bucket Sort
* Heap
* Frequency Counting
