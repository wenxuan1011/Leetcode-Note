# 278. First Bad Version

`Binary Search`, `Interactive`

## Problem

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

### Example

```
Input: n = 5, bad = 4
Output: 4

Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

```
Input: n = 1, bad = 1
Output: 1
```

### Constraints
* `1 <= bad <= n <= 2^31 - 1`
---

## Idea

### 1. Binary Search

用 binary search 的 code 去改寫：

1. `if isBadVersion(mid) == False` 時 `left = mid + 1`，因為 mid 不為答案，所以 left 要往後再推一個。
2. `if isBadVersion(mid) == True` 時 `right = mid`，因為 mid 很可能就是答案，若像 binary search 一樣為 `right = mid - 1` 的話可能會不小心從 true 變 false，所以 right 不用往前推一個。
3. 因為有了上面的改動，while loop 的中指條件不再是發生在 `left > right` 的情況，而會是在 `left = right` 時停止 (**表示 left 逼近到 第一個 true 發生的位置**)，因此要改成 `left < right`。
4. 也因為第三點的關係，**我們要回傳得值為 left**，非 mid。

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left = 1
        right = n
        while(left<right):
            mid = (left+right) // 2
            if isBadVersion(mid) == False:
                left = mid + 1
            else:
                right = mid
        return left
```
* Time complexity: $O(log\space n)$
* Space complexity: $O(1)$