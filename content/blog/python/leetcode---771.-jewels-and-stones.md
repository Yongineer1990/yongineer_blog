---
title: Leetcode - 771. Jewels and Stones
date: 2021-08-12 01:08:20
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

`jewels`는 보석이며 `stones`는 갖고있는 돌이다. `stones`중 보석의 개수를 구하라. 단, 대소문자는 구분한다.

### Example 1.

```python
Input: jewels = "aA", stones = "aAAbbbb"
Output: 3
```

### Example 2.

```python
Input: jewels = "z", stones = "ZZ"
Output: 0
```

# 나의 풀이

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        d = {}

        for s in stones:
            if s not in d.keys():
                d[s] = 1
            else:
                d[s] += 1

        num = 0
        for j in jewels:
            if j in d.keys():
                num += d[j]

        return num
```

![https://yongineer.duckdns.org/python/leetcode_771_1.png](https://yongineer.duckdns.org/python/leetcode_771_1.png)

먼저 `dictionary`자료형 변수 `d`를 선언해주고 `stones`의 각 문자를 꺼내와서 만약 `d`에 존재하지 않는 문자라면 키로 추가해주고 값을 `1`생성한다. 만약 이미 존재하는 문자(키)라면 해당 키에 값을 `1`올려준다.

이렇게 생성한 `d`에 이번엔 `jewels`의 각 문자를 꺼내와 매칭되는 키의 값을 모두 더한뒤 리턴한다.

# 풀이 : defaultdict

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        d = collections.defaultdict(int)
        num = 0

        for s in stones:
            d[s] += 1

        for j in jewels:
            num += d[j]

        return num
```

![https://yongineer.duckdns.org/python/leetcode_771_2.png](https://yongineer.duckdns.org/python/leetcode_771_2.png)

앞선 `dictionary`자료형인 변수 `d`를 존재하지 않는 키를 자동으로 생성해주는 `collections.defaultdict(int)`로 선언하여 키 존재 여부를 체크 하지 않는 풀이이다.

# 풀이 : Counter

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        d = collections.Counter(stones)
        num = 0

        for j in jewels:
            num += d[j]

        return num
```

![https://yongineer.duckdns.org/python/leetcode_771_3.png](https://yongineer.duckdns.org/python/leetcode_771_3.png)

앞서 `stone`의 각 문자를 계산하는 로직이 존재했는데 이를 `collection.Counter(stones)`로 자동으로 처리해주었다. 또한 `Counter`는 존재하지 않는 키를 참조할때 `KeyError`를 발생시키지 않고 0을 리턴하기 때문에 `collections.defaultdict`와 동일하게 에러에 대한 예외처리를 할 필요가 없다.

# 풀이 : Pythonic Way

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        return sum(s in jewels for s in stones)
```

![https://yongineer.duckdns.org/python/leetcode_771_4.png](https://yongineer.duckdns.org/python/leetcode_771_4.png)

...?

이 풀이를 쉽게 이해 하기 위해서는 각 단계에 존재하는 `List comprehension`을 출력해 볼 필요가 있다.

```python
>>> [s for s in stones]
['a', 'A', 'A', 'b', 'b', 'b', 'b']
```

여기까지는 각 문자를 하나씩 출력하는 `List comprehension`이다.

```python
>>> [s in jewels for s in stones]
[True, True, True, False, False, False, False]
```

이제 여기에 비교구문을 추가하여 `jewels`에 포함된 문자면 `True`, 아니라면 `False`를 반환하게 한다.

이 결과를 `sum`하면 되는데 [Python의 `sum`은 인수로 `iterator`를 받으므로](https://docs.python.org/ko/3/library/functions.html?highlight=built%20function#sum) `List comprehension`을 의미하는 대괄호(`[]`)는 생략 가능하다.
