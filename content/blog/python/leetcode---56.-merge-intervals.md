---
title: Leetcode - 56. Merge Intervals
date: 2021-08-13 17:08:86
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)


# 문제

겹치는 구간을 병합하라.

### Example 1.

```python
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

### Example 2.

```python
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

# 나의 풀이

```python
from operator import itemgetter

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals = sorted(intervals, key=itemgetter(0))

        res = []
        for i in intervals:
            if res and i[0] <= res[-1][1]:
                res[-1][1] = max(i[1], res[-1][1])
            else:
                res.append(i)

        return res
```

![https://yongineer.duckdns.org/python/leetcode_56_1.png](https://yongineer.duckdns.org/python/leetcode_56_1.png)

먼저 주어진 `List`를 정렬한다 이때, 주어진 `List`는 중첩 리스트이고 이중 첫번째 값을 기준으로 정렬해야 하므로 `operator.itemgetter`를 이용하였다.

그리고 병합을 진행할 빈 리스트 (`res`)를 생성하고 정렬된 리스트 (`intervals`)에 대해 반복을 수행한다.

반복을 수행하면서 `res`가 존재하고 현재 아이템의 시작 값(`i[0]`)이 이전 아이템(`res[-1]`)의 마지막 값보다 작거나 같다면(겹친다면) 이전 아이템의 마지막 값을 이 두 아이템의 마지막 값중 최대값으로 갱신 시켜준다.

만약 조건에 포함되지 않는다면 `res`에 정렬된 주어진 리스트의 현재 아이템(`i`)을 적재한다.

이렇게 비교 및 병합 과정이 끝나면 새롭게 생성된 병합된 리스트 `res`를 리턴해준다.

# 풀이 : 정렬하여 병합

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        merged = []

        for i in sorted(intervals, key=lambda x: x[0]):
            if merged and i[0] <= merged[-1][1]:
                merged[-1][1] = max(merged[-1][1], i[1])
            else:
                merged += i,

        return merged
```

![https://yongineer.duckdns.org/python/leetcode_56_2.png](https://yongineer.duckdns.org/python/leetcode_56_2.png)

풀이는 나의 풀이와 거의 동일하지만 특이한 문법이 하나 있다.

## 콤마(`,`)연산자

단순히 `+=`연산을 했을때는 요소를 이어 붙인다.

```python
>>> a = [1]
>>> b = [2,3]
>>> a += b
>>> a
[1, 2, 3]
```

행렬의 연결 연산과 동일하다. 반면 `,`연산을 한다면

```python
>>> a = [1]
>>> b = [2,3]
>>> a += b,
>>> a
[1, [2, 3]]
```

중첩 리스트가 된다. `,`는 중첩 리스트로 만들어주는 역할을 하며 이는 `[]`를 부여한 것과 동일하다.

```python
>>> a = [1]
>>> b = [2,3]
>>> a += [b]
>>> a
[1, [2, 3]]
```
