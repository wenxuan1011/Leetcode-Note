# 125. Valid Palindrome

`Two Pointers`, `String`

## Problem

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome***, or `false` *otherwise*.


### Example

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome. (不分大小寫)
```

```
Input: s = " "
Output: true
```

### Constraints
* `1 <= s.length <= 2 * 10^5`
* `s` consists only of printable ASCII characters.

---

## Idea

### 1. Two Pointers - Clean First

先把非字母的符號、空格刪除，再由頭尾一起比對，向中間逼近。

* **Note:**
    * `char.lower()` : 將字串或字元整理成小寫 (`upper()` 為大寫)
    * `char.isalnum()` : 檢查字元是否為字母，用來濾除符號、空格
    

```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        s_list = [char.lower() for char in s if char.isalnum()]
        front = 0
        end = len(s_list)-1
        while(front < end):
            if s_list[front] != s_list[end]:
                return False
            front += 1
            end -= 1
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$
    * 一開始整理 `s_list` 的空間複雜度


### 2. Two Pointer - without Cleaning

去掉一開始把非字母的符號、空格刪除建置 list 的過程。(因為耗費 memory)

直接在 while loop 裡面檢查是否為字母，若不是字母則往下推進。(反正時間複雜度一樣為 $O(n)$)

```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        front, end = 0, len(s)-1
        while(front < end):
            if not s[front].isalnum():
                front += 1
            elif not s[end].isalnum():
                end -= 1
            else:
                if s[front].lower() != s[end].lower():  
                    return False
                front += 1
                end -= 1
        return True
```
* Time complexity: $O(n)$
* Space complexity: $O(1)$

### 3. Brute-force

整理完後，直接 reverse 整個 list。

如果 origin 和 reverse 相同，則為倒裝。