# 167. Two Sum II - Input Array Is Sorted

## Pattern

Two Pointers

## Problem

Given a **1-indexed** array of integers `numbers` that is sorted in non-decreasing order, find two numbers such that they add up to a specific `target`.

Return the indices (1-indexed) of the two numbers.

There is exactly one solution, and you may not use the same element twice.

---

## Constraints

```text
2 <= numbers.length <= 3 × 10^4
-1000 <= numbers[i] <= 1000
numbers is sorted in non-decreasing order.
-1000 <= target <= 1000
Exactly one valid answer exists.
```

---

# Approach 1: Brute Force

## Idea

Check every possible pair and return the pair whose sum equals the target.

---

## Algorithm

1. Pick the first element.
2. Pair it with every element after it.
3. If their sum equals the target, return the indices.

---

## Code

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        n = len(numbers)

        for i in range(n - 1):
            for j in range(i + 1, n):
                if numbers[i] + numbers[j] == target:
                    return [i + 1, j + 1]
```

---

## Dry Run

```python
numbers = [2,7,11,15]
target = 9
```

Compare:

```text
2 + 7 = 9 ✓
```

Return

```python
[1,2]
```

---

## Complexity Analysis

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(1)
```

---

# Why Improve?

The array is already **sorted**.

Can we use this property instead of checking every pair?

Yes.

---

# Approach 2: Two Pointers (Optimal)

## Key Observation

Since the array is sorted:

* If the current sum is **too small**, we need a **larger** number.
* If the current sum is **too large**, we need a **smaller** number.

Because the array is sorted:

* Moving the **left pointer** increases the sum.
* Moving the **right pointer** decreases the sum.

Therefore, we can eliminate many unnecessary comparisons.

---

## Why Does This Work?

Suppose:

```text
numbers = [2,7,11,15]
target = 9
```

Initially:

```text
2                15
^                 ^
l                 r
```

Current sum:

```text
17
```

Too large.

Since the array is sorted:

Moving the left pointer would only make the sum larger.

So the only useful move is:

```text
Move right pointer ←
```

Now:

```text
2          11
^           ^
```

Still too large.

Move right again.

```text
2     7
^     ^
```

Now:

```text
2 + 7 = 9
```

Found the answer.

---

## Algorithm

1. Place one pointer at the beginning.
2. Place another pointer at the end.
3. Calculate the current sum.
4. If:

   * Sum equals target → return the indices.
   * Sum is smaller than target → move the left pointer.
   * Sum is greater than target → move the right pointer.
5. Repeat until the answer is found.

---

## Code

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1

        while l < r:
            currSum = numbers[l] + numbers[r]

            if currSum > target:
                r -= 1

            elif currSum < target:
                l += 1

            else:
                return [l + 1, r + 1]
```

---

## Dry Run

```python
numbers = [2,7,11,15]
target = 9
```

| Left | Right | Sum | Action       |
| ---- | ----: | --: | ------------ |
| 2    |    15 |  17 | Move Right   |
| 2    |    11 |  13 | Move Right   |
| 2    |     7 |   9 | Answer Found |

Return:

```python
[1,2]
```

---

## Complexity Analysis

### Time Complexity

```text
Each pointer moves at most n times.

Overall : O(n)
```

### Space Complexity

```text
O(1)
```

---

# Why Not Use a HashMap?

A HashMap also gives an `O(n)` solution.

However, since the array is already sorted, the Two Pointer approach:

* Uses constant extra space.
* Takes advantage of the sorted order.
* Is simpler and more efficient.

---

# Comparison

| Approach     | Time  | Space |
| ------------ | ----- | ----- |
| Brute Force  | O(n²) | O(1)  |
| HashMap      | O(n)  | O(n)  |
| Two Pointers | O(n)  | O(1)  |

---

# Pattern Signal

Ask yourself:

```text
Is the array sorted?

↓

Need to find a pair?

↓

Can moving one pointer increase/decrease the answer?

↓

Two Pointers
```

---

# Interview Flow

```text
Brute Force

↓

Notice the array is sorted.

↓

Large sum?
Move right.

↓

Small sum?
Move left.

↓

Two Pointers (Optimal)
```

---

# Learnings

* A sorted array often eliminates the need for a HashMap.
* Two pointers work because moving one pointer changes the sum predictably.
* Always ask whether the input order can be exploited before introducing extra space.
* This problem is the foundation for understanding **3Sum**, where we fix one element and solve the remaining part using the same Two Pointer technique.

---

# Tags

* Array
* Two Pointers
* Sorted Array
* Binary Search Alternative
