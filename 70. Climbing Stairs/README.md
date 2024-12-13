# 70. Climbing Stairs

`Math`, `Dynamic Programming`, `Memoization`

## Problem

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

### Example

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

### Constraints
* `1 <= n <= 45`

---

## Idea

### 1. Intuition - Recursive (TLE)

n 的答案其實就是 (n-1 時的解全部都多走一步) + (n-2 時的解全部都多走兩步)。

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n <= 2:
            return n
        return climbStairs(n-1) + climbStairs(n-2)
```
* Time complexity: $O(2^n)$
    * The recursively break-down sub-problems can be represented by a binary tree, which can take $O(2^n)$ to traverse.
* Space complexity: $O(n)$
    * The depth of call stack takes $O(n)$.

### 2. Intuition - For Loop

把解法一的概念換成用 for loop 慢慢加。

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n <= 3:
            return n

        prev_1 = 3
        prev_2 = 2
        cur = 0

        for i in range(3, n):
            cur = prev_1 + prev_2
            prev_2 = prev_1
            prev_1 = cur
            
        return cur
```
* Time complexity: $O(n)$
* Space complexity: $O(1)$