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

### 1. Recursive (TLE)

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

### 2. For Loop

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

### 3. Recursive with Memoization

建立一個 `memory` 的 Dict，將有算過的結果都記下來，可以不用像方法一重複計算。

使時間複雜度由 $O(2^n)$ 變成 $O(n)$。

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        def _climbStairs(n):
            if n in memory:
                return memory[n]
            memory[n] = _climbStairs(n-1) + _climbStairs(n-2)
            return memory[n]
        
        memory = {1: 1, 2: 2}
        return _climbStairs(n)
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$
    * The depth of call stack takes $O(n)$.

### 4. 1d Dynamic Programming (DP) with DP Array

用一個 list 一個一個去算 `list[i] = list[i-1] + list[i-2]`。

由 i＝2 一路算到 i=n+1，即得到答案。

* **Note:**
    * counting：[1, 1, 2, 3, 5, 8, ...]
    * index：[0, 1, 2, 3, 4, 5, ...]
    * **注意：n=0 的情況雖然不存在，但為了讓 n=2 後都能按照規律計算，n=0 的初始值設定為 1。**

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        counting_list = [1] * (n+1)
        for i in range(2, n+1):
            counting_list[i] = counting_list[i-1] + counting_list[i-2]
        return counting_list[n]
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$
