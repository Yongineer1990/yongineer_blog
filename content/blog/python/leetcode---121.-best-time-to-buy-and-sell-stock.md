---
title: Leetcode - 121. Best Time to Buy and Sell Stock
date: 2021-08-09 02:08:78
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

한 번의 거래로 낼 수 있는 최대 이익을 산출하라

### Example 1.

```python
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

### Example 2.

```python
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

# 나의 풀이 : Two Pointer (실패)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        start = 0
        end = len(prices) - 1

        val = 0
        while start < end:
            diff = prices[end] - prices[start]

            if diff < 0:
                start += 1

            elif diff > val:
                val = diff
                end -= 1

            elif diff < val:
                start += 1
                end -= 1

        return val
```

![https://yongineer.duckdns.org/python/leetcode_121_1.png](https://yongineer.duckdns.org/python/leetcode_121_1.png)

일단 나는 `Two pointer`알고리즘을 사용하였다. 팔 시점(`end`)에서 산 시점 (`start`)의 가격의 차이(`diff`)를 구한다 이때 `diff`가 0보다 작다면 손해가 난 결과이므로 `start`를 우측으로 이동(증가)시킨다.

```python
while start < end:
		diff = prices[end] - prices[start]

		if diff < 0:
				start += 1
		...
```

만약 최고가(`val`)보다 `diff`가 크다면 `diff`를 `val`로 저장하고 `end`를 좌측으로 이동(감소)시킨다.



```python
while start < end:
		...

		elif diff > val:
				val = diff
				end -= 1
		...
```

만약 `val`보다 `diff`가 작다면 손해가 발생했으므로 `start`와 `end`모두 각각 좌 우측으로 이동시킨다.

```python
while start < end:
		...

		elif diff < val:
				start += 1
				end -= 1
		...
```

이렇게 거래 기간중 최고가 `val`을 리턴하였다. 당연히 성공한줄 알았지만... `Timeout`이 발생하였다.

![https://yongineer.duckdns.org/python/leetcode_121_2.png](https://yongineer.duckdns.org/python/leetcode_121_2.png)

# 풀이 : 저점과 현재 값과의 차이 계산

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        min_price = sys.maxsize

        for price in prices:
            min_price = min(min_price, price)
            profit = max(profit, price - min_price)

        return profit
```

![https://yongineer.duckdns.org/python/leetcode_121_3.png](https://yongineer.duckdns.org/python/leetcode_121_3.png)

내가 너무 복잡하게 문제를 생각하는것 같았다. 이 풀이에서는 단순하게 현재 값이 우측으로 이동하면서 저점을 갱신시키고 (`min_price`) 저점과 현재 값을 비교하여 차이를 계산하였다 (`profit`)

이때 `min_price`는 초기값으로 변수의 최대값(`sys.maxsize`)으로 지정한다. (만약, 변소의 최소값으로 저장이 필요한 경우 : `-sys.maxsize`)
