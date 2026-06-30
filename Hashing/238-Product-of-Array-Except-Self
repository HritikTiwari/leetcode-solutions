# 238. Product of Array Except Self

## Pattern

Prefix Sum / Postfix Sum (Prefix & Suffix Products)

## Problem

Given an integer array `nums`, return an array `answer` such that:

```text
answer[i] = product of all elements of nums except nums[i]
```

You must solve it **without using division** and in **O(n)** time.

---

## Constraints

```text
2 <= nums.length <= 10^5
-30 <= nums[i] <= 30

The product of any prefix or suffix fits in a 32-bit integer.
```

---

# Approach 1: Brute Force

## Idea

For every index, multiply every other element except itself.

---

## Algorithm

1. Iterate through every index.
2. For each index, traverse the entire array.
3. Skip the current index.
4. Multiply all remaining elements.

---

## Code

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result = []

        for i in range(len(nums)):
            product = 1

            for j in range(len(nums)):
                if i != j:
                    product *= nums[j]

            result.append(product)

        return result
```

---

## Dry Run

```python
nums = [1,2,3,4]
```

```text
i = 0
2 × 3 × 4 = 24

i = 1
1 × 3 × 4 = 12

i = 2
1 × 2 × 4 = 8

i = 3
1 × 2 × 3 = 6
```

Return

```python
[24,12,8,6]
```

---

## Complexity Analysis

### Time Complexity

```text
Outer Loop : O(n)

Inner Loop : O(n)

Overall : O(n²)
```

### Space Complexity

```text
O(1)
```

(excluding the output array)

---

# Why Improve?

For every element, we repeatedly multiply the same numbers.

Can we reuse previously computed products?

Yes.

---

# Approach 2: Prefix & Postfix Arrays

## Idea

For every index:

```text
Product Except Self

=

Prefix Product × Postfix Product
```

Example:

```text
nums = [1,2,3,4]

answer[2]

=

(1 × 2)

×

(4)

=

8
```

We can precompute:

* Prefix products
* Postfix products

and multiply them together.

---

## Complexity

```text
Time : O(n)

Space : O(n)
```

because two extra arrays are used.

---

# Can We Improve Further?

Notice:

We don't actually need to store both prefix and postfix arrays.

We can:

* Store prefix products directly inside the answer array.
* Keep only one postfix variable while traversing backwards.

This reduces the extra space to **O(1)**.

---

# Approach 3: Prefix + Postfix (Optimal)

## Key Observation

For every index:

```text
answer[i]

=

(Product of all elements before i)

×

(Product of all elements after i)
```

Instead of storing two arrays:

* Store prefix products inside the answer array.
* Compute postfix products on the fly.

---

## Algorithm

### First Pass (Left → Right)

Store the prefix product before each index.

### Second Pass (Right → Left)

Maintain a postfix product.

Multiply the postfix with the prefix already stored in the answer array.

---

## Code

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result = [1] * len(nums)

        prefix = 1
        for i in range(len(nums)):
            result[i] = prefix
            prefix *= nums[i]

        postfix = 1
        for j in range(len(nums) - 1, -1, -1):
            result[j] *= postfix
            postfix *= nums[j]

        return result
```

---

## Dry Run

```python
nums = [1,2,3,4]
```

### Prefix Pass

Initially

```text
result = [1,1,1,1]
prefix = 1
```

| i | Prefix | Result    |
| - | ------ | --------- |
| 0 | 1      | [1,1,1,1] |
| 1 | 1      | [1,1,1,1] |
| 2 | 2      | [1,1,2,1] |
| 3 | 6      | [1,1,2,6] |

After first pass

```python
result = [1,1,2,6]
```

---

### Postfix Pass

Initially

```text
postfix = 1
```

| j | postfix | result      |
| - | ------- | ----------- |
| 3 | 1       | [1,1,2,6]   |
| 2 | 4       | [1,1,8,6]   |
| 1 | 12      | [1,12,8,6]  |
| 0 | 24      | [24,12,8,6] |

Return

```python
[24,12,8,6]
```

---

## Complexity Analysis

### Time Complexity

```text
Prefix Traversal : O(n)

Postfix Traversal : O(n)

Overall : O(n)
```

### Space Complexity

```text
O(1)
```

excluding the output array.

The `result` array does not count as extra space because it is required for the output.

---

# Why This Works

For every index:

```text
answer[i]

=

Prefix Product

×

Postfix Product
```

The first traversal stores the prefix.

The second traversal multiplies it with the postfix.

Thus, every element is processed only twice.

---

# Comparison

| Approach                  | Time     | Space    |
| ------------------------- | -------- | -------- |
| Brute Force               | O(n²)    | O(1)     |
| Prefix + Postfix Arrays   | O(n)     | O(n)     |
| Prefix + Postfix Variable | **O(n)** | **O(1)** |

---

# Interview Flow

```text
Brute Force

↓

Prefix & Postfix Arrays

↓

Reuse the output array and one postfix variable (Optimal)
```

Implement the **Prefix + Postfix Variable** solution.

---

# Learnings

* Prefix and postfix products allow reuse of previously computed values.
* The output array can often be reused to reduce auxiliary space.
* When a problem forbids division, think about **prefix/suffix computations**.
* The output array is not counted as extra space in this problem.

---

# Tags

* Array
* Prefix Sum
* Suffix Sum
* Prefix Product
* Dynamic Programming (Technique)
