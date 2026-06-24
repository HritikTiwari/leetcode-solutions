# 242. Valid Anagram

## Pattern

HashMap / Frequency Counting

## Problem

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An anagram is a word or phrase formed by rearranging the letters of another word, using all the original letters exactly once.

---

## Constraints

```text
1 <= s.length, t.length <= 5 * 10^4
s and t consist of lowercase English letters.
```

---

# Approach 1: Sorting

## Idea

If two strings are anagrams, then after sorting, both strings become identical.

## Algorithm

1. Sort both strings.
2. Compare the sorted strings.
3. If they are equal, return `True`; otherwise, return `False`.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```

## Dry Run

```python
s = "anagram"
t = "nagaram"
```

After sorting:

```python
sorted(s) = ['a', 'a', 'a', 'g', 'm', 'n', 'r']
sorted(t) = ['a', 'a', 'a', 'g', 'm', 'n', 'r']
```

Return:

```python
True
```

## Complexity Analysis

### Time Complexity

```text
Sorting s : O(n log n)
Sorting t : O(n log n)

Overall : O(n log n)
```

### Space Complexity

```text
O(n)
```

because sorting creates additional space in Python.

---

# Approach 2: HashMap (Optimal)

## Key Observation

Two strings are anagrams if and only if:

```text
Every character appears the same number of times in both strings.
```

Therefore, instead of sorting, we can store the frequency of each character.

---

## Idea

1. If lengths are different, return `False`.
2. Create two frequency maps.
3. Count the frequency of every character in both strings.
4. Compare the frequencies.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)

        for c in countS:
            if countS[c] != countT.get(c, 0):
                return False

        return True
```

## Dry Run

```python
s = "anagram"
t = "nagaram"
```

Frequency maps:

```python
countS = {
    'a': 3,
    'n': 1,
    'g': 1,
    'r': 1,
    'm': 1
}

countT = {
    'n': 1,
    'a': 3,
    'g': 1,
    'r': 1,
    'm': 1
}
```

All frequencies match.

Return:

```python
True
```

---

## Complexity Analysis

### Time Complexity

```text
Building frequency maps : O(n)
Comparing frequencies   : O(n)

Overall : O(n)
```

### Space Complexity

```text
O(n)
```

---

# Python Shortcut: Counter

The same frequency-counting approach can be written more concisely using `Counter`.

## Code

```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

## Complexity Analysis

```text
Time  : O(n)
Space : O(n)
```

---

# Follow-Up (Lowercase English Letters Only)

Since the strings contain only lowercase English letters, we can use a frequency array of size 26.

## Code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count = [0] * 26

        for i in range(len(s)):
            count[ord(s[i]) - ord('a')] += 1
            count[ord(t[i]) - ord('a')] -= 1

        return all(x == 0 for x in count)
```

## Complexity Analysis

```text
Time  : O(n)
Space : O(1)
```

The space complexity is `O(1)` because the array size is fixed at 26 and does not depend on the input size.

---

# Comparison

| Approach                   | Time       | Space |
| -------------------------- | ---------- | ----- |
| Sorting                    | O(n log n) | O(n)  |
| HashMap                    | O(n)       | O(n)  |
| Counter                    | O(n)       | O(n)  |
| Frequency Array (26 chars) | O(n)       | O(1)  |

---

# Interview Flow

```text
Naive:
Repeatedly search/remove characters -> O(n²)

↓

Sorting:
Sort both strings and compare -> O(n log n)

↓

HashMap:
Count character frequencies -> O(n)

↓

Follow-up:
Use an array of size 26 -> O(n) time and O(1) space
```

Implement the **HashMap** solution unless the interviewer specifically asks for further optimization.

---

# Learnings

* If a problem asks whether two strings contain the same characters with the same frequencies, think about **frequency counting**.
* Sorting is often a good first optimization.
* HashMaps are useful for counting arbitrary characters.
* Fixed character sets can often be optimized to **constant space** using arrays.

---

# Tags

* String
* Hashing
* Frequency Counting
* Arrays
