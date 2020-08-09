---
title: TIL.34) Coding Test - 로그 파일 재정렬
date: 2020-08-09 20:08:40
category: Python
thumbnail: '../TIL/images/python.png'
draft: false
---

![](../TIL/images/python.png)

이 문제는 LeetCode에 소개된 문제이며 [여기](https://leetcode.com/problems/reorder-data-in-log-files/)에서 문제를 풀어 볼 수 있습니다.

# Question

로그를 재정렬하라. 기준은 다음과 같다

1. 로그의 가장 앞 부분은 식별자다.
2. 문자로 구성된 로그가 숫자 로그보다 앞에 온다.
3. 식별자는 순서에 영행을 끼치지 않지만, 문자가 동일할 경우 식별자 순으로 한다.
4. 숫자 로그는 입력 순서대로 한다.

**Example 1:**

```python
Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
```

# 나의 해결법

```python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        logdict = {}
        digit = []
        char = []
        
        for i in logs:
            log = i.split()
            logdict[log[0]] = log[1:]
            if logdict[log[0]][0].isdigit():
                digit.append(i)
            else:
                char.append(i)
                
        char.sort(key=lambda x: (x.split()[1:], x.split()[0]))
        
        return char + digit
```

![https://yongnas.iptime.org/python/til34_1.png](https://yongnas.iptime.org/python/til34_1.png)

`logs`의 각 요소를 `split()`함수로 분리 한 뒤 그 첫번째 값을 키로 하는 딕셔너리 형태로 저장한 다음 해당 키의 값이 숫자면 (`isdigit()`) `digit`이라는 빈 리스트에 저장하고 문자라면 `char`라는 빈 리스트에 하나씩 저장한다.

그 다음 문자열 값만 갖고 있는 `char`을 정렬하는데(`sort()`) 이때 `lambda`식을 이용하여 기준값을 `split()`함수로 분리 한 뒤 식별자를 제외 한 값을 첫번째 기준으로 하고 만약 동일한 경우 식별자를 두번째 기준으로 지정 하여 정렬한 다음 리스트 연산자 `+`를 이용하여 문자열 값 + 수치형 값 형태로 합친 다음 반환하였다.
