# 125. Valid Palindrome

## Pattern

Two Pointers

## Problem

A phrase is a palindrome if, after converting all uppercase letters to lowercase and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.

---

## Constraints

```text
1 <= s.length <= 2 × 10^5
s consists of printable ASCII characters.
```

---

# Approach 1: Build a New String (Brute Force)

## Idea

Ignore all non-alphanumeric characters, convert the remaining characters to lowercase, build a new string, and compare it with its reverse.

---

## Algorithm

1. Create an empty string.
2. Traverse the original string.
3. If the character is alphanumeric:

   * Convert it to lowercase.
   * Append it to the new string.
4. Compare the new string with its reverse.
5. Return the result.

---

## Code

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        newStr = ""

        for c in s:
            if c.isalnum():
                newStr += c.lower()

        return newStr == newStr[::-1]
```

---

## Dry Run

```python
s = "A man, a plan, a canal: Panama"
```

Filtered string:

```text
amanaplanacanalpanama
```

Reverse:

```text
amanaplanacanalpanama
```

Both are equal.

Return:

```python
True
```

---

## Complexity Analysis

### Time Complexity

```text
Building string : O(n)
Reverse string  : O(n)

Overall : O(n)
```

### Space Complexity

```text
O(n)
```

because an extra string is created.

---

# Why Improve?

We don't actually need another string.

We can compare characters directly from both ends.

---

# Approach 2: Two Pointers (Optimal)

## Key Observation

A palindrome has matching characters from both ends.

Instead of creating another string:

* Skip non-alphanumeric characters.
* Compare valid characters directly.
* Move both pointers toward the center.

This avoids using extra space.

---

## Algorithm

1. Initialize two pointers:

   * `l = 0`
   * `r = len(s) - 1`
2. Move `l` forward until it points to an alphanumeric character.
3. Move `r` backward until it points to an alphanumeric character.
4. Compare both characters after converting them to lowercase.
5. If they differ, return `False`.
6. Otherwise, move both pointers inward.
7. Repeat until the pointers meet or cross.
8. Return `True`.

---

## Code

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:

            while l < r and not self.alnum(s[l]):
                l += 1

            while l < r and not self.alnum(s[r]):
                r -= 1

            if s[l].lower() != s[r].lower():
                return False

            l += 1
            r -= 1

        return True

    def alnum(self, c):
        return (
            ord('A') <= ord(c) <= ord('Z') or
            ord('a') <= ord(c) <= ord('z') or
            ord('0') <= ord(c) <= ord('9')
        )
```

---

## Dry Run

```python
s = "A man, a plan, a canal: Panama"
```

Initial:

```text
l → 'A'
r → 'a'
```

Compare:

```text
a == a ✓
```

Skip spaces and punctuation.

Next valid comparison:

```text
m == m ✓
```

Continue until the pointers meet.

Return:

```python
True
```

---

## Why `while l < r`?

The pointers move toward each other from opposite ends.

Once they meet (odd-length string) or cross (even-length string), every required comparison has already been completed.

The middle character of an odd-length palindrome never needs to be compared with itself.

---

## Complexity Analysis

### Time Complexity

```text
Each character is visited at most once.

Overall : O(n)
```

### Space Complexity

```text
O(1)
```

No additional string or array is created.

---

# Comparison

| Approach         | Time | Space |
| ---------------- | ---- | ----- |
| Build New String | O(n) | O(n)  |
| Two Pointers     | O(n) | O(1)  |

---

# Interview Flow

```text
Build a filtered string

↓

Compare with reverse

↓

Two Pointers:
Compare characters directly while skipping non-alphanumeric characters
```

Implement the **Two Pointers** solution.

---

# Learnings

* Two pointers are ideal when comparing elements from both ends of a sequence.
* Skip unnecessary characters before comparison.
* `l < r` ensures comparisons stop once the pointers meet or cross.
* Eliminating the extra string reduces the auxiliary space from `O(n)` to `O(1)`.

---

# Tags

* String
* Two Pointers
* Palindrome
