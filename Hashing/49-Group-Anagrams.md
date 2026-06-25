# 49. Group Anagrams

## Pattern

HashMap + Frequency Counting

## Problem

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An anagram is a word formed by rearranging the letters of another word using all the original letters exactly once.

---

## Constraints

```text
1 <= strs.length <= 10^4
0 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.
```

---

# Approach 1: Sorting

## Idea

Two strings are anagrams if their sorted versions are identical.

Examples:

```text
eat -> aet
tea -> aet
ate -> aet
```

All three belong to the same group.

---

## Algorithm

1. Create an empty HashMap.
2. For every string:

   * Sort the string.
   * Use the sorted string as the key.
   * Append the original string to the corresponding list.
3. Return all values of the HashMap.

---

## Code

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        result = defaultdict(list)

        for s in strs:
            key = "".join(sorted(s))
            result[key].append(s)

        return list(result.values())
```

---

## Dry Run

```python
strs = ["eat","tea","tan","ate","nat","bat"]
```

```text
"eat" -> "aet"
result = {"aet": ["eat"]}

"tea" -> "aet"
result = {"aet": ["eat","tea"]}

"tan" -> "ant"
result = {
    "aet": ["eat","tea"],
    "ant": ["tan"]
}

"ate" -> "aet"
result = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan"]
}

"nat" -> "ant"
result = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan","nat"]
}

"bat" -> "abt"
result = {
    "aet": ["eat","tea","ate"],
    "ant": ["tan","nat"],
    "abt": ["bat"]
}
```

Return:

```python
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

---

## Complexity Analysis

Let:

* `N` = number of strings
* `K` = maximum length of a string

### Time Complexity

```text
Sorting one string = O(K log K)

For N strings:

O(N × K log K)
```

### Space Complexity

```text
O(N × K)
```

for storing the grouped strings.

---

# Why Improve?

Sorting every string costs:

```text
O(K log K)
```

Can we identify anagrams without sorting?

Yes.

Two strings are anagrams if they have the same character frequencies.

---

# Approach 2: Frequency Count (Optimal)

## Key Observation

All anagrams have the same frequency of characters.

Example:

```text
eat -> [1,0,0,...,1,...,1]
tea -> [1,0,0,...,1,...,1]
ate -> [1,0,0,...,1,...,1]
```

Therefore, we can use the frequency array as the key.

Since lists are mutable and cannot be dictionary keys, convert it to a tuple.

---

## Algorithm

1. Create a HashMap.
2. For every string:

   * Build a frequency array of size 26.
   * Convert it to a tuple.
   * Use the tuple as the key.
   * Append the string to that group.
3. Return all grouped anagrams.

---

## Code

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        result = defaultdict(list)

        for s in strs:
            count = [0] * 26

            for c in s:
                count[ord(c) - ord('a')] += 1

            result[tuple(count)].append(s)

        return list(result.values())
```

---

## Dry Run

```python
strs = ["eat","tea","tan","ate","nat","bat"]
```

Frequency keys:

```text
eat -> (1,0,0,0,1,...,1,...)
tea -> (1,0,0,0,1,...,1,...)
ate -> (1,0,0,0,1,...,1,...)

tan -> (1,0,...,1,...,1,...)
nat -> (1,0,...,1,...,1,...)

bat -> (1,1,0,...,1,...)
```

HashMap:

```python
{
    key1: ["eat","tea","ate"],
    key2: ["tan","nat"],
    key3: ["bat"]
}
```

Return:

```python
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

---

## Complexity Analysis

Let:

* `N` = number of strings
* `K` = maximum length of a string

### Time Complexity

Building frequency arrays:

```text
O(K) per string
```

For all strings:

```text
O(N × K)
```

### Space Complexity

```text
O(N × K)
```

for storing the grouped strings.

---

# Why `tuple(count)`?

This will fail:

```python
result[count].append(s)
```

because:

```text
TypeError: unhashable type: 'list'
```

Lists are mutable and cannot be dictionary keys.

This works:

```python
result[tuple(count)].append(s)
```

because tuples are immutable and hashable.

---

# Why `list(result.values())`?

```python
result.values()
```

returns:

```python
dict_values([...])
```

But LeetCode expects:

```python
List[List[str]]
```

Therefore:

```python
return list(result.values())
```

---

# Comparison

| Approach        | Time           | Space    |
| --------------- | -------------- | -------- |
| Sorting         | O(N × K log K) | O(N × K) |
| Frequency Count | O(N × K)       | O(N × K) |

---

# Interview Flow

```text
Brute Idea:
Compare every string with every other string and check if they are anagrams.

↓

Sorting:
Use sorted strings as keys.

↓

Optimal:
Use character frequency arrays as keys.
```

Implement the **frequency count solution**.

---

# Learnings

* Grouping problems often use a HashMap.
* Frequency counting can replace sorting.
* Tuples can be dictionary keys; lists cannot.
* `dict_values` is not the same as `List`.

---

# Tags

* Array
* String
* Hashing
* HashMap
* Frequency Counting
