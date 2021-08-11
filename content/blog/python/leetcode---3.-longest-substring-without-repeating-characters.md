---
title: Leetcode - 3. Longest Substring Without Repeating Characters
date: 2021-08-12 02:08:02
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

# 문제

중복 문자가 없는 가장 긴 부분 문자열의 길이를 리턴하라

### Example 1.

```python
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### Example 2.

```python
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3.

```python
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Example 4.

```python
Input: s = ""
Output: 0
```

# 풀이 : Sliding Window & Two Poinrter

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        d = {}
        res = start = 0

        for i, c in enumerate(s):
            if c in d and start <= d[c]:
                start = d[c] + 1
            else:
                res = max(res, i - start + 1)

            d[c] = i

        return res
```

![https://yongineer.duckdns.org/python/leetcode_3_1.png](https://yongineer.duckdns.org/python/leetcode_3_1.png)

먼저 Two Poinrter로 출발하지만 두개의 포인터 모두 왼쪽에서 부터 출발한다. 각각 왼쪽 시작점에서 출발해 두번째 포인터(`i`)는 다음과 같이 계속 오른쪽으로 확장한다.

```python
...
for i, c in enumerate(s):
		if c in d:
				start = d[c] + 1
		else:
				...
```

만약 여기서 포인터가 만난적 있는 문자라면 `d`에 있을것이고 이 경우 첫번째 포인터인 `start`를 `d[c] + 1`위치로 갱신한다. 만약 만난적 없던 문자라면 다음과 같이 처리한다.

```python
for i, c in enumerate(s):
		if c in d:
				...
		else:
				res = mas(res, i - start + 1)
```

처음 보는 문자인 경우 매번 `max()`를 통해 길이를 확인하면서 더 큰값인 경우 갱신해준다.

도한 다음과 같이 현재 문자의 위치는 계속 갱신해준다.

```python
for i, c in enumerate(s):
		if c in d:
				...
		else:
				...

		d[c] = i
```

이제 `d[c]`는 현재 문자를 키로 하는 해시 테이블이며 여기에는 현재 위치를 값으로 삽입한다. 즉 앞서 `start = d[c] + 1`은 현재위치의 +1이고 이미 등장했던 문자의 경우 `start`를 현재 위치까지 옮기게 된다.

그런데 이미 등장했다고 무작정 옮겨버린다면 sliding window 바깥에 있는 문자라도 이미 등장한 적이 있어 위치가 옮겨지게 된다. 이를 방지 하기위해 sliding window 바깥에 있는 문자는 무시를 해줘야 한다. 따라서 다음과 같이 비교 구문을 추가해야 한다.

```python
for i, c in enumerate(s);
		if c in d and start <= d[c]:
				...
		else:
				...
```
