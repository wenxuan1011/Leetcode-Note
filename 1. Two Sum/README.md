# 1. Two Sum

`Array`, `Hash Table`

## Problem

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.


### Example

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

### Constraints
* `2 <= nums.length <= 10^4`
* `-10^9 <= nums[i] <= 10^9`
* `-10^9 <= target <= 10^9`
* Only one valid answer exists.

---

## Idea

### 1. Brute-force

從一開始的 element 開始向後比對，看漢神相加會符合 target。 有找到就 return，沒有就繼續。（`nums` 沒有按大小排列）

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```
* Time complexity: $O(n^2)$
    * 最慘就是全部都要檢查
* Space complexity: $O(1)$

### 2. Hash table

順著計算每個 element 和 target 的差值，然後返回去 hash table 裡面找有沒有該差值的紀錄。

hash table 裡面存 element 的 value 和 index。

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hash_table = {}
        for i, num in enumerate(nums):
            if target - num in hash_table:
                return [i, hash_table[target - num]]
            hash_table[num] = i

```
* Time complexity: $O(n)$
    * 只需要比 n 次 (n 個 elements)
* Space complexity: $O(n)$
    * hash table 最多要存 n 個 key & value