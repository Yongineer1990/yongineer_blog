---
title: Leetcode - 973. K Closest Points to Origin
date: 2021-08-13 19:08:65
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

평면상에 points 목록이 있을 때, 원점 (0.0)에서 K번 가까운 점 목록을 순서대로 출력하라. 평면상 두 점의 거리는 유클리드 거리로 한다.

### Example 1.

![https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

```python
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

### Example 2.

```python
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

# 나의 풀이

```python
from math import dist

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        top = {}
        zero = (0,0)

        for i in points:
            key = dist(zero, i)

            if key in top.keys():
                key += 0.1

            top[key] = i

        top = collections.OrderedDict(sorted(top.items()))
        res = [v for _, v in top.items()]

        return res[:k]
```

![https://yongineer.duckdns.org/python/leetcode_973_1.png](https://yongineer.duckdns.org/python/leetcode_973_1.png)

원점에서의 유클리드 거리를 계산하기 위해 Python의 내장 함수 `math.dist`를 사용하였다. 이 결과를 `dictionary`타입의 변수 `top`의 `key`로 사용하였고 `value`는 주어진 리스트의 각 요소가 된다. 만약 동일한 결과가 발생하였을 경우 `key`값을 `0.1`올려 주었다.

이제 이렇게 생성된 `top`을 `key`값을 기준으로 정렬하였고 이중 `value`만 따로 `List Comprehension`으로 추출하였다. (`res`) 이때 이미 `key`를 기준으로 정렬되어 마찬가지로 `res`에도 가까운 순으로 추출이된다. 이 `res`를 주어진 상위 개수 만큼만 반환한다.

# 풀이 : Queue

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        heap = []

        for (x, y) in points:
            dist = x ** 2 + y ** 2
            heapq.heappush(heap, (dist, x, y))

        result = []
        for _ in range(k):
            (dist, x, y) = heapq.heappop(heap)
            result.append((x, y))

        return result
```

이 풀이에서는 유클리드 거리 계산을 위해 `math.sqrt`를 사용하였다. 또한 우선순위 queue를 구현하기 위해 heap에 삽입 하였는데 과정은 다음과 같다.

```python
heap = []
for (x, y) in points:
		dist = math.sqrt((0 - x) ** 2 + (0 - y) ** 2)
		heapq.heappush(heap, (dist, x, y))
```

Python의 `heapq`모듈은 최소 힙이기 때문에 거리가 가까운 순을 출력해야하는 이 문제 풀이에 적합하다. 만약 가장 먼 거리를 출력해야 한다면 음수로 변환하여 `-dist`를 삽입하는 형태로 풀이 할 수있다.

이제 결과를 추출한다. 결과 리스트에 힙에서 추출한 결과를 `k`번 삽입해 리턴한다.

```python
result = []
for _ in range(k):
		(dist, x, y) = heapq.heappop(heap)
		result.append((x, y))

return result
```

다만 여기서 `math.sqrt()`는 삭제해도 무방하다.

```python
dist = x ** 2 + y ** 2
```

그러나 내 풀이보다 속도 면에서 더 느렸다.... ~~나의 승리인가...✌️~~
