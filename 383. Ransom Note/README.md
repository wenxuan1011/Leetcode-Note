# 383. Ransom Note

`Hash Table`, `String`, `Counting`

## Problem

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

### Example

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

### Constraints
* `1 <= ransomNote.length, magazine.length <= 10^5`
* `ransomNote` and `magazine` consist of lowercase English letters.

---

## Idea

> 直接參考 **242. Valid Anagram** 的解法

### 1. Hash Table

(也可以用 ASCII 的版本)

```python
class Solution(object):
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        if len(ransomNote) > len(magazine):
            return False
        hash_table = {}
        for char in magazine:
            hash_table[char] = hash_table.get(char, 0) + 1
        for char in ransomNote:
            if char in hash_table and hash_table[char] != 0:
                hash_table[char] -= 1
            else:
                return False
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(26) \to O(1)$
    * 最多建 26 個英文字母的 hash table

### 2. Count Characters Number

1. 可以用 `set()` 來得出 `ransomNote` 中有那些英文字母

2. 並且 string 可以使用 `.count()` 來計算該 char 在 string 中的數量，以此比較是否滿足 `ransomNote.count(char) <= magazine.count(char)`

```python
class Solution(object):
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        if len(ransomNote) > len(magazine):
            return False
        for char in set(ransomNote):
            if ransomNote.count(char) > magazine.count(char):
                return False
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(26) \to O(1)$