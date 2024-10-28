# 121. Best Time to Buy and Sell Stock

`Array`, `Dynamic Programming`

## Problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve* from this transaction. If you cannot achieve any profit, return `0`.


### Example

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
```

```
Input: prices = [7,6,4,3,1]
Output: 0
```

### Constraints
* `1 <= prices.length <= 10^5`
* `0 <= prices[i] <= 10^4`

---

## Idea

### 1. Brute-force (Time Limit Exceeded)

就是逐一爆力解。(耗時太長，error)

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        max_profit = 0
        for i in range(len(prices)):
            for j in range(i+1, len(prices)):
                max_profit = max(max_profit, prices[j]-prices[i])
        return max_profit
```
* Time complexity: $O(n^2)$
* Space complexity: $O(1)$


### 2. Kadane's Algorithm (Dynamic Programming)

只要執行過一次 list 中所有的 element 就可以得到答案。

因為題目一定要拿後面的減掉前面的，所以只要一直去紀錄**過去最小的是誰 (min_price)**，然後拿當前的 price 減去 min_price，再和目前儲存的最大獲利比較。

依序更新就可得到結果。

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        max_profit = 0
        min_price = float('inf')
        for p in prices:
            min_price = min(min_price, p)
            max_profit = max(max_profit, p-min_price)
        return max_profit
```
* Time complexity: $O(n)$
* Space complexity: $O(1)$