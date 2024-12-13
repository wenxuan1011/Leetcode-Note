# 242. Valid Anagram

`Hash Table`, `String`, `Sorting`

## Problem

Given two strings `s` and `t`, return `true` if `t` is an 
**anagram** of `s`, and `false` otherwise.

* **anagram :**
    * 相同字母異序詞（兩個單字用到的字母與次數一模一樣，只是調換順序）

### Example

```
Input: s = "anagram", t = "nagaram"
Output: true
```

```
Input: s = "rat", t = "car"
Output: false
```

### Constraints
* `1 <= s.length, t.length <= 5 * 10^4`
* `s` and `t` consist of lowercase English letters.

---

## Idea

### 1. Hash Table

一開始要先檢查兩個單字的長度是否一致。

用一個 dict 紀錄 `s` 中所有字母出現的次數，再用 `t` 中的字母依序去檢查 **是否可以在 dict 中查到** 且 **查到時還有餘量 (每次檢查到後都要 -1 更新餘量)**。

* **Note:**
    * `get(key[, default])` 是 Python dict 物件的內建函式：
        * 如果 dict 本身沒有該 key 時會出現 **KeyError**，用 `get()` 則不會
        * 如果 dict 內 key 存在，回傳查詢到的 value
        * 如果 dict 內 key 不存在，回傳 default 設置的值
        * default 是用來設定預設值的可選引數，若未指定則為 None
    

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
            
        hash_table = {}
        for char in s:
            hash_table[char] = hash_table.get(char, 0) + 1
        for char in t:
            if char in s and hash_table[char] != 0:
                hash_table[char] -= 1
            else:
                return False
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(26) \to O(1)$
    * 最多建 26 個英文字母的 hash table


### 2. Hash Table - ASCII Value

和 1. 的概念完全一樣，只是建一個 list 表示 A~Z 字母的出現次數。

**這樣可以不用每次都檢查 `t` 的字母有沒有出現在 `s` 中**

* **Note :**
    * `ord(char)` : returns the ASCII value of the character.
    * `ord(char) - ord('a')` : index of A~Z.


```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False

        count = [0] * 26
        for char in s:
            count[ord(char)-ord('a')] += 1
        for char in t:
            if count[ord(char)-ord('a')] != 0:
                count[ord(char)-ord('a')] -= 1
            else:
                return False
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(26)\to O(1)$

### 3. Two Hash Table

概念和上面完全一樣，只是改成存兩個 hash table，然後比較他們有沒有長得一樣。

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False

        HashTable_s = {}
        HashTable_t ={}
        for char_s, char_t in zip(s, t):
            HashTable_s[char_s] = HashTable_s.get(char_s, 0) + 1
            HashTable_t[char_t] = HashTable_t.get(char_t, 0) + 1 
        return HashTable_s == HashTable_t
```
* Time complexity: $O(n)$
* Space complexity: $O(26)+O(26)\to O(1)$

### 4. Sorting

用 pyhton 的 `sorted()` 可以完成字串排序 (按字母順序排)

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False

        s_list = sorted(s)
        t_list = sorted(t)

        return s_list == t_list
```
* Time complexity: $O(n\ log\ n)$
    * Sorting takes $O(n\ log\ n)$.
* Space complexity: $O(n)$
    * `sorted` returns a list of characters.