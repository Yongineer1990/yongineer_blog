---
title: Leetcode - 561. Array partition 1
date: 2021-08-09 00:09:47
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

n개의 페어를 이용한 `min(a, b)`의 합으로 만들 수 있는 가장 큰 수를 출력하라.

### Example 1.

```python
Input: nums = [1,4,3,2]
Output: 4
Explanation: All possible pairings (ignoring the ordering of elements) are:
1. (1, 4), (2, 3) -> min(1, 4) + min(2, 3) = 1 + 2 = 3
2. (1, 3), (2, 4) -> min(1, 3) + min(2, 4) = 1 + 2 = 3
3. (1, 2), (3, 4) -> min(1, 2) + min(3, 4) = 1 + 3 = 4
So the maximum possible sum is 4.
```

### Example 2.

```python
Input: nums = [6,2,6,5,1,2]
Output: 9
Explanation: The optimal pairing is (2, 1), (2, 5), (6, 6). min(2, 1) + min(2, 5) + min(6, 6) = 1 + 2 + 6 = 9.
```

# 나의 풀이

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        nums.sort()
        chunk = [nums[i:i+2] for i in range(0, len(nums), 2)]

        num = 0
        for i in chunk:
            num += min(i[0], i[1])

        return num
```

![https://yongineer.duckdns.org/python/leetcode_561_1.png](https://yongineer.duckdns.org/python/leetcode_561_1.png)

먼저 주어진 배열을 정렬한다. 그리고 나서 이를 페어로 쪼갠다. 이때, `List Comprehension`을 사용했다. 이렇게 페어로 구성된 2차원 리스트를 `loop`돌려 각 요소의 첫번째와 두번째 요소의 `min`을 구해 `num`변수에 더해주었다.

# 풀이 : Pythonic Way

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        return sum(sorted(nums)[::2])
```

![https://yongineer.duckdns.org/python/leetcode_561_2.png](https://yongineer.duckdns.org/python/leetcode_561_2.png)

어차피 이 문제는 오름차순으로 정렬된 리스트에서 짝수번째의 요소만 더해주면 결과는 항상 페어중 작은 것들의 합과 동일하다. (0부터 시작하므로) 따라서 `List slicing`을 이용하여 주어진 리스트를 정렬하고 슬라이싱 구문 `[::2]`를 사용하여 2칸씩 건너띄며 모든 요소를 더해준다면 결과는 짝수번째의 요소를 모두 더한것과 같다.
