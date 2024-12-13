# 409. Longest Palindrome

`Hash Table`, `String`, `Greedy`

## Problem

Given a string `s` which consists of lowercase or uppercase letters, return the length of the **longest** 
**palindrome** that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome.


### Example

```
Input: s = "abccccdd"
Output: 7
Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.
```

### Constraints
* `1 <= s.length <= 2000`
* `s` consists of lowercase **and/or** uppercase English letters only.

---

## Idea

### 1. Hash Table

先建立一個 hash table 記錄 `s` 中所有字母出現的次數，個數為偶數者可以全使用，個數為奇數者只能使用偶數個的部分 (例如：‘b’ 有 5 個，那只能使用  4 個 'b')。

且為了最大化長度，若有奇數個字母存在時，長度可以再 **+1** (可以將落單的字母擺放在字串正中間)。

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash_table = {}
        for char in s:
            hash_table[char] = hash_table.get(char, 0) + 1
        length = 0
        odd = False
        for key in hash_table:
            if hash_table[key] % 2 == 0:
                length += hash_table[key]
            else:
                length += hash_table[key] - 1
                odd = True
        if odd == True:
            length += 1
        return length
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$


### 2. Check for Pairs in Every Loop

1. 以一個 set 記錄目前出現過的字母 (set 運算的速度比 list 快很多)

2. 如果同一個字母揍成一對 (出現兩次) 時，即表示可以放入排列中，故 `length += 2`，且要刪去 set 中的字母以免重複計算。

3. 最後檢查 set 中是否還有落單的字母，有的話 `length + 1`。

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        char_set = set()
        length = 0
        for char in s:
            if char in char_set:
                length += 2
                char_set.remove(char)
            else:
                char_set.add(char)
        if char_set:
            length += 1
        return length
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$

### 3. Count Odd

在建立 hash table 時就計算總共有幾個字母的數量為奇數，可以組成回文的字母個數即為 `len(s) - (odd count) + 1` (此為 `odd count > 1` 的情況)。

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        odd_count = 0
        hash_table = {}
        for char in s:
            hash_table[char] = hash_table.get(char, 0) + 1
            if hash_table[char] % 2 == 0:
                odd_count -= 1
            else:
                odd_count += 1
        if odd_count > 1:
            return len(s) - odd_count + 1
        return len(s)
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$