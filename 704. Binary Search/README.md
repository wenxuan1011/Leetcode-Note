# 704. Binary Search

`Array`, `Binary Search`

## Problem

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

### Example

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
```

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
```

### Constraints
* `1 <= nums.length <= 104`
* `-10^4 < nums[i], target < 10^4`
* All the integers in `nums` are **unique**.
* `nums` is sorted in ascending order.

---

## Idea

### 1. Iterative

用  `left` `right`  去逼近要搜尋的答案，以左右邊界的中間值 (`mid`) 去決定如何調整邊界。

* **Note:**
    * 在 while loop 的部分一定要將判斷是寫成 `left <= right`，若只寫 `left < right` 在某些情況會陷入無限迴圈。
    * 邊界移動時一定要寫成 `left = mid+1` 和 `right = mid-1`，在以下情況才不會卡住：
        ```
        nums = [2,5], target = 5
        ```
      有了這樣的機制才會促使在下一個迴圈時變成 `left=right=mid`，得以繼續執行。
      
      且若發生 target 皆不再 list 中的情況時，可以藉由發生 `left > right` 的判斷結束迴圈的執行。
    
```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        left = 0
        right = len(nums)-1
        while(left<=right):
            mid = (left+right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1
```
* Time complexity: $O(log\space n)$
    * As binary search keeps halving the input array, it takes $O(log\space n)$ to find `target`.
* Space complexity: $O(1)$


### 2. Recursive

Just change while loop to recursive.

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        def BinarySearch(left, right):
            if left > right:
                return -1
            mid = (left+right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                return BinarySearch(mid+1, right)
            else:
                return BinarySearch(left, mid-1)

        return BinarySearch(0, len(nums)-1)
```
* Time complexity: $O(log\space n)$
* Space complexity: $O(log\space n)$
    * The depth of call stack takes $O(log\space n)$.
